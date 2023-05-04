# Translatior's Guide to Project: Eden's Garden
A list of tools and tutorials to make translations of the Project: Eden's Garden Prologue.

## Getting started
In order to implement your translation into the game, you will need:
* UABEA
* dnSpy

## How to find and translate the text
### Dialogues
### Examinable Items
### Menu UI
#### Increasing Text Margins
### Game Saves
#### Slot, Time and Date
```csharp
private void FoundSave(SaveSlot slot, int i)
{
	// Translate these strings:
	string i18n-saveTitle = "Slot {0} : {1}"; // This is the title of the save. {0} is the slot number. {1} is the .
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
		text4 = string.Format("({0})", str);
	}
	return string.Format(i18n-locationTitle, text, text2, text4);
  ```
#### Missing Room Titles
