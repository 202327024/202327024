var sheetId = SpreadsheetApp.openById("1fCVm056B8BZsfHwziRahhxSUNiiI4hDCWeGIMQvqsNE");
var sheet = sheetId.getSheets()[0];
var p;
var result, msg, value;

function response() {
  var json = {};
  json.order = p.order;
  json.result = result;
  json.msg = msg;
  json.value = value;

  var jsonData = JSON.stringify(json);
  return ContentService.createTextOutput(jsonData).setMimeType(ContentService.MimeType.JSON);
}

function setResult(_result, _msg) {
  result = _result;
  msg = _msg;
}

function register() {
  var cell = sheet.getRange(2, 1, sheet.getLastRow(), 1).getValues();
  if (cell.some(row => row[0] == p.id)) {
    setResult("Error", "이미 존재하는 이름입니다.");
    return;
  }
  sheet.appendRow([p.id, p.playdate]);
  setResult("OK", "회원가입 완료");
}

function register2() {
  var lastRow = sheet.getLastRow(); // 마지막 행을 가져옴
  var cell2 = sheet.getRange(2, 3, lastRow - 1, 1).getValues(); // C2 열부터 마지막 행까지의 데이터를 가져옴
  var nextRow = cell2.filter(String).length + 2; // C열의 데이터 수에 따라 다음 행 번호 결정

  var range = sheet.getRange(nextRow, 3, 1, 2); // C열(D열 포함)
  range.setValues([[p.num_ball, p.num_percentage]]);
  setResult("OK", "데이터 추가 완료");
}

function register3() {
  var lastRow2 = sheet.getLastRow(); // 마지막 행을 가져옴
  var cell3 = sheet.getRange(2, 5, lastRow2 - 1, 1).getValues(); // E2 열부터 마지막 행까지의 데이터를 가져옴
  var nextRow = cell3.filter(String).length + 2; // E열의 데이터 수에 따라 다음 행 번호 결정

  var range2 = sheet.getRange(nextRow, 5, 1, 1); // E열
  range2.setValues([[p.success_ball]]);
  setResult("OK", "데이터 추가 완료");
}

function doPost(e) {
  p = e.parameter;
  switch(p.order) {
    case "register":
      register();
      break;
    case "register2":
      register2();
      break;
    case "register3":
      register3();
      break;
  }
  return response();
}
