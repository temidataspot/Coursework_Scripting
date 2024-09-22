# Tracking Stuents' Reading Times

It is important to note that this project was carried out in the Google Environment using AppScript

```javascript
var SPREADSHEET_ID = 'GoogleSheet_ID'; 

function logStartTime(email) {
  var sheet = SpreadsheetApp.openById(SPREADSHEET_ID).getSheetByName('Sheet1');
  sheet.appendRow([email, new Date(), '']);
}

function logEndTime(email) {
  var sheet = SpreadsheetApp.openById(SPREADSHEET_ID).getSheetByName('Sheet1');
  var data = sheet.getDataRange().getValues();
  for (var i = 0; i < data.length; i++) {
    if (data[i][0] == email && data[i][2] == '') {
      sheet.getRange(i + 1, 3).setValue(new Date());
      break;
    }
  }
}

function onOpen() {
  var ui = DocumentApp.getUi();
  ui.createMenu('Track Reading')
    .addItem('Start Reading', 'startReading')
    .addItem('End Reading', 'endReading')
    .addToUi();
}

function startReading() {
  var email = Session.getActiveUser().getEmail();
  logStartTime(email);
  DocumentApp.getUi().alert('Start time logged for ' + email);
}

function endReading() {
  var email = Session.getActiveUser().getEmail();
  logEndTime(email);
  DocumentApp.getUi().alert('End time logged for ' + email);
}
```
This script ws run on the ```onOpen``` function to authorize the script. It was then deployed as a web app 
to track student reading times and integrated into the classroom
