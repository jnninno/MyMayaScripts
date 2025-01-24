import maya.cmds as cmds

def create_and_snap_locator():
    """
    Creates a locator. If an object is selected, snaps the locator 
    to the object's world position *and rotation* by using a parent 
    constraint (deleted afterward).
    Then prompts the user to rename the locator (default name: loc_01).
    """
    # Store the current selection
    sel = cmds.ls(selection=True)

    # Create a locator at origin (Maya defaults to origin if no position is given)
    locator_name = cmds.spaceLocator(name="loc_01")[0]  # [0] is the transform node
    
    # If there is a selection, snap the locator to the first selected object
    if sel:
        # Create a parent constraint (no offset) to match translation & rotation
        constraint = cmds.parentConstraint(sel[0], locator_name, mo=False)
        
        # Delete the constraint so the locator is "free" but remains snapped
        cmds.delete(constraint)
    
    # Prompt the user for a new locator name
    result = cmds.promptDialog(
        title="Rename Locator",
        message="Enter New Locator Name:",
        button=["OK", "Cancel"],
        defaultButton="OK",
        cancelButton="Cancel",
        dismissString="Cancel",
        text="loc_01"   # default text
    )
    
    if result == "OK":
        new_name = cmds.promptDialog(query=True, text=True)
        if new_name.strip():
            # Rename the locator transform node to whatever was entered
            locator_name = cmds.rename(locator_name, new_name.strip())

create_and_snap_locator()
