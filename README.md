# Translatior's Guide to Project: Eden's Garden
A list of tools and tutorials to make translations of the Project: Eden's Garden Prologue.

## Getting started
In order to implement your translation into the game, you will need:
* [UABEA](https://github.com/nesrak1/UABEA/releases/tag/v6)
* [dnSpy](https://github.com/dnSpy/dnSpy/releases/tag/v6.1.8)

## How to find and translate the text
### Table of contents:
* [Dialogues](#dialogues)
* [Examinable items](#examinable-items)
* [Menu UI](#menu-ui)
* [Game Saves](#game-saves)
### Dialogues
### Examinable Items
#### Normal Items
#### Doors :(
Some are in the `Door` files, some are in the `GYM_<room name>` files. It's a bit of a mess.
#### Characters
### Menu UI
#### Increasing Text Margins
### Game Saves
#### Slot, Time and Date
```csharp
private void FoundSave(SaveSlot slot, int i)
{
	// Translate these strings:
	string i18n-saveTitle = "Slot {0} : {1}"; // This is the title of the save. {0} is the slot number. {1} is the chapter and location.
	string i18n-saveDate = "{1}/{0}/{2}"; // This is the date of the save. {0} = day, {1} = month, {2} = year.
	string i18n-saveTime = "{0}:{1}:{2}" // This is the time of the save. {0} = hours, {1} = minutes, {2} = seconds.
	// ------------------------
	
	slot.HasData = true;
	try
	{
		GameData data = GameSaver.GetData("save" + i.ToString());
		slot.Title.text = string.Format(i18n-saveTitle,
                                                (i + 1).ToString(),
						this.EvaluateSaveTitle(data));
		slot.Date.text = string.Format(i18n-saveDate,
                                               data.BaseData.Date.Day.ToString().PadLeft(2, '0'),
                                               data.BaseData.Date.Month.ToString().PadLeft(2, '0'),
                                               data.BaseData.Date.Year.ToString());
		slot.TimeOfSave.text = string.Format(i18n-saveTime,
                                                     data.BaseData.Date.Hour.ToString().PadLeft(2, '0'),
                                                     data.BaseData.Date.Minute.ToString().PadLeft(2, '0'),
                                                     data.BaseData.Date.Second.ToString().PadLeft(2, '0'));
	}
	catch
	{
		Debug.LogError("Slot BaseSave data could not be read and written to the GUI");
	}
}
```
#### Room Data
```csharp
{
	// Translate these strings:
	string i18n-locationTitle = "{0} {1} {2}" // The title of the location part of a save file.
	                                          // {0} is the abbreviation of the chapter name;
	                                          // {1} is the name of the room;
	                                          // {2} is the current state of the game, which can be either nothing or the i18n-saveState string.
					      
	string i18n-saveState = "({0})" // The state of the game. {0} can be replaced with one of the following strings.
	string i18n-saveState-investigation = "Investigation"; // This state indicates an ongoing investigation.
	string i18n-saveState-preparation = "Preparation"; // This state indicates game saves made from the prompt before the "Class Trial Preparations" menu.
	string i18n-saveState-conclusion = "Conclusion"; // This state indicates the part of the game after a CLass Trial.
	string i18n-saveState-END = "END"; // This state indicates game saves made from the (nonexisting) prompt at the end of a chapter.
	// ------------------------
	
	string text = "";
	string text2 = s.BaseData.SceneName.Replace("GYM_", "");
	string text3 = "";
	string text4 = "";
	if (s.MainData.location != null && s.MainData.location != "")
	{
		text2 = s.MainData.location;
	}
	if (ProgressionManager.instance != null)
	{
		text = ProgressionManager.instance.GetChapterSaveTitle(s.ProgressionData.chapter);
	}
	if (s.MainData.addedState != AddState.None)
	{
	      	string str;
		switch (s.MainData.addedState)
		{
		case AddState.Investigation:
			str = i18n-saveState-investigation;
			break;
		case AddState.Preparation:
			str = i18n-saveState-preparation;
			break;
		case AddState.Conclusion:
			str = i18n-saveState-conclusion;
			break;
		case AddState.END:
			str = i18n-saveState-END;
			break;
		default:
			str = s.MainData.addedState.ToString();
			break;
		}
		text4 = string.Format(i18n-saveState, str);
	}
	return string.Format(i18n-locationTitle, text, text2, text4);
}
  ```
#### Missing Room Titles

Some rooms take their name directly from the name of the scene. To solve this, you need to complicate your life.

You need to add three assets: one with Type Number/ID: `GameObject`, one with Type Number/ID: `Transform`, and one with Type Number/ID: `MonoBehaviour`.

Type Number/ID: `GameObject` (Path ID: `NUM_1`)
```
0 GameObject Base
 0 vector m_Component
  1 Array Array (2 items)
   0 int size = 2
   [0]
    0 ComponentPair data
     0 PPtr<Component> component
      0 int m_FileID = 0
      0 SInt64 m_PathID = NUM_2
   [1]
    0 ComponentPair data
     0 PPtr<Component> component
      0 int m_FileID = 0
      0 SInt64 m_PathID = NUM_3
 0 unsigned int m_Layer = 0
 1 string m_Name = "Location"
 0 UInt16 m_Tag = 0
 0 bool m_IsActive = true
 ```
 
 Type Number/ID: `Transform` (Path ID: `NUM_2`)
 ```
 0 Transform Base
 0 PPtr<GameObject> m_GameObject
  0 int m_FileID = 0
  0 SInt64 m_PathID = NUM_1
 0 Quaternionf m_LocalRotation
  0 float x = 0
  0 float y = 0
  0 float z = 0
  0 float w = 1
 0 Vector3f m_LocalPosition
  0 float x = 0
  0 float y = 0
  0 float z = 0
 0 Vector3f m_LocalScale
  0 float x = 1
  0 float y = 1
  0 float z = 1
 0 vector m_Children
  1 Array Array (0 items)
   0 int size = 0
 0 PPtr<Transform> m_Father
  0 int m_FileID = 0
  0 SInt64 m_PathID = 0
```

Type Number/ID: `MonoBehaviour` (Path ID: `NUM_3`)
```
0 MonoBehaviour Base
 0 PPtr<GameObject> m_GameObject
  0 int m_FileID = 0
  0 SInt64 m_PathID = NUM_1
 1 UInt8 m_Enabled = 1
 0 PPtr<MonoScript> m_Script
  0 int m_FileID = 1
  0 SInt64 m_PathID = 594
 1 string m_Name = ""
 1 string title = "Location Title"
```
