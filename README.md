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
#### Room Data
```csharp
{
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
			str = "Indagini";
			break;
		case AddState.Preparation:
			str = "Preparativi";
			break;
		case AddState.Conclusion:
			str = "Conclusione";
			break;
		case AddState.END:
			str = "FINE";
			break;
		default:
			str = s.MainData.addedState.ToString();
			break;
		}
		text4 = string.Format("({0})", str);
	}
	return string.Format("{0} {1} {2}", text, text2, text4);
  ```
#### Missing Room Titles
#### Time Data
```csharp
private void FoundSave(SaveSlot slot, int i)
{
	slot.HasData = true;
	try
	{
		GameData data = GameSaver.GetData("save" + i.ToString());
		slot.Title.text = string.Format("Slot {0} : {1}",
      (i + 1).ToString(), this.EvaluateSaveTitle(data)
    );
		slot.Date.text = string.Format("{0}/{1}/{2}",
                                   data.BaseData.Date.Day.ToString().PadLeft(2, '0'),
                                   data.BaseData.Date.Month.ToString().PadLeft(2, '0'),
                                   data.BaseData.Date.Year.ToString());
		slot.TimeOfSave.text = string.Format("{0}:{1}:{2}",
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
