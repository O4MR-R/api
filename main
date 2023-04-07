from flask_cors import CORS
import gspread
import os
from oauth2client.service_account import ServiceAccountCredentials
from flask import Flask, request, jsonify

app = Flask(__name__)
CORS(app)

# Set the credentials file and sheet name
credentials_file = 'test.json'
spreadsheet_name = 'Winners - AR'

# Authenticate with the Google Sheets API
scope = ['https://spreadsheets.google.com/feeds',
         'https://www.googleapis.com/auth/drive']
credentials = ServiceAccountCredentials.from_json_keyfile_name(
    credentials_file, scope)
gc = gspread.authorize(credentials)

# Find the spreadsheet by name
spreadsheet = None
for s in gc.openall():
  if s.title == spreadsheet_name:
    spreadsheet = s
    break

if spreadsheet is None:
  print(f'No spreadsheet found with name "{spreadsheet_name}"')
else:
  # Select the first sheet
  sheet = spreadsheet.get_worksheet(0)



@app.route('/')
def search():
    value_to_search = request.args.get('value')
    records = sheet.get_all_records()
    lower_records = [{key.lower(): value.lower() for key, value in record.items()} for record in records]
    results = [record for record in lower_records if value_to_search.lower() in record.values()]
    if len(results)>0:
        result = 'Yes'
    else:
        result = 'No'
    return jsonify({'result': result})



if __name__ == '__main__':
  os.system (" clear")
  app.run('0.0.0.0',port=80)
