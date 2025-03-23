# Excel file in sharepoint to Batabase using Azure Data Factory

For this project, the most important thing is to access the Excel files stored in SharePoint and be able to read them correctly, then transfer them to a table within a database for future transformations.

Within Azure Data Factory, the first step is to create a Python file that allows reading files from SharePoint using the following code:

from office365.runtime.auth.authentication_context import AuthenticationContext
from office365.sharepoint.client_context import ClientContext
from office365.sharepoint.files.file import File
import pandas as pd


```python
URL = 'https://sharepoint.com/sites/Site_Name' #Your Site Name
USUARIO = 'User_Name' #Your user name
PASSWORD = 'Password' #Your password

def autenticacion():
    ctx_auth = AuthenticationContext(URL)
    ctx_auth.acquire_token_for_user(USUARIO, PASSWORD)  
    ctx = ClientContext(URL, ctx_auth)
    return ctx

misitio = autenticacion()
print(misitio)
datos = File.open_binary(misitio, "/sites/DataAnalytics/Shared Documents/File/Test.xlsx") #Your file

with open('Prueba.xlsx', 'wb') as archivo_local:
    archivo_local.write(datos.content)
   
df = pd.read_excel('Test.xlsx', sheet_name='Sheet1')
