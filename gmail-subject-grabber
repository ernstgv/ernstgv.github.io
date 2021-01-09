from __future__ import print_function
import pickle
import os.path
from googleapiclient.discovery import build
from google_auth_oauthlib.flow import InstalledAppFlow
from google.auth.transport.requests import Request
import time
# If modifying these scopes, delete the file token.pickle.
SCOPES = ['https://www.googleapis.com/auth/gmail.readonly']
def main():
    """Shows basic usage of the Gmail API.
    Lists the user's Gmail labels.
    """
    creds = None
    # The file token.pickle stores the user's access and refresh tokens, and is
    # created automatically when the authorization flow completes for the first
    # time.
    if os.path.exists('token.pickle'):
        with open('token.pickle', 'rb') as token:
            creds = pickle.load(token)
    # If there are no (valid) credentials available, let the user log in.
    if not creds or not creds.valid:
        if creds and creds.expired and creds.refresh_token:
            creds.refresh(Request())
        else:
            flow = InstalledAppFlow.from_client_secrets_file(
                'credentials.json', SCOPES)
            creds = flow.run_local_server(port=0)
        # Save the credentials for the next run
        with open('token.pickle', 'wb') as token:
            pickle.dump(creds, token)
    service = build('gmail', 'v1', credentials=creds)
    # Call the Gmail API
    z = time.time()
    currentlocaltime = time.strftime("%Y/%m/%d", time.localtime())
    w = z - 86400
    yesterdaytime = time.strftime("%Y/%m/%d", time.localtime(w))
    teammembers = ["firstname.lastname","emailusername","firstpartofemailaddress"]
    for eachmember in teammembers:
        print("")
        print(eachmember)
        searchquery = "after:{0} before:{1} from:{2}@yourdomain.com".format(yesterdaytime,currentlocaltime, eachmember)
        results = service.users().messages().list(q = searchquery, userId='me').execute()
        messageslist = results.get('messages')
        
        #succeeding lines came from https://www.geeksforgeeks.org/how-to-read-emails-from-gmail-using-gmail-api-in-python/
        
        for singlemessage in messageslist:
            txt = service.users().messages().get(userId='me', id=singlemessage['id']).execute()
            try:
                payload = txt['payload']
                headers = payload['headers']
                for d in headers:
                    if d['name'] == 'Subject':
                        subject = d['value']
                print(subject)
            except:
                pass

if __name__ == '__main__':
    main()
