# Tracking Access and Submission Times

This code logs the time students access a form and submits. Student email are automatically captured
in the Google environment.

```javascript
// Function to handle web app requests and redirect to the form
function doGet(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Start Times');
  var email = e.parameter.email; // Capture email from the web app URL
  var timestamp = new Date();
  
  // Log the email and start time to the Start Times sheet
  if (email) {
    sheet.appendRow([email, timestamp]);
  } else {
    sheet.appendRow([null, timestamp]);
  }
  
  // Redirect to the Google Form
  var formUrl = "Google_Form_Url"; 
  return HtmlService.createHtmlOutput('<html><body><script>window.location.href="' + formUrl + '";</script></body></html>');
}

// Function to handle form submissions
function onFormSubmit(e) {
  var formResponsesSheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Form Responses'); // Sheet with form responses
  var startTimesSheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Start Times'); // Sheet to log start times

  var responses = e.values; // Form responses
  var email = responses[1]; // Email is in the second column, adjust index if necessary
  var submissionTime = new Date();

  // Log email and start time to Start Times sheet
  startTimesSheet.appendRow([email, submissionTime]);
}

// Function to set up the trigger for form submissions
function setupTrigger() {
  ScriptApp.newTrigger('onFormSubmit')
    .forSpreadsheet(SpreadsheetApp.getActiveSpreadsheet())
    .onFormSubmit()
    .create();
}
```
This code adds the ```doget``` function, run the ```setupTrigger``` function that will call the ```onFormSubmit``` function
when the form is submitted
