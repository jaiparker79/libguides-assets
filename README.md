# LibGuides Assets link checking tool

This PowerShell script checks URLs for Assets exported in CSV format from the
LibGuides content management system.  It was made by Jai Parker, Information
Access Librarian at the Queensland University of Technology with help from
Microsoft Copilot.  As per the [license](./LICENSE), caveat emptor.

## Prerequisites
PowerShell requires: 
1. an execution policy set to run this script and
2. installation of the Import Excel cmdlet to perform the cancelled item checking.

To set the Execution Policy:
1. Click the Windows start button and search for "Windows PowerShell" then select **Run as administrator**
2. Type `Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser` and press Enter
3. When asked "Do you want to change the execution policy?" press Y
4. Type `Install-Module -Name ImportExcel -Scope CurrentUser` and press Enter
5. Follow any prompts to authorise installation of a 3rd party/non-Microsoft cmdlet

## Running the script
To run this script:

1. Download a csv of Assets for checking from Talis. The CSV file must have the name assets_list.csv for the script to pick them up.
2. If you are running a search for links of cancelled Alma items add a file named cancelled.xlsx containing the direct export of the Alma Portfolios which have been deleted from the catalogue.
 **Note:** the cancelled items checking works because MMS IDs are included in Primo Permalinks eg. qut.primo.exlibrisgroup.com/permalink/61QUT_INST/1g7tbfa/alma**991010993492104001**
3. Rename the file cancelled.xlsx and save it to the same folder as assets_list.csv

### Input

URLs are fed into the script via CSV files. Files must be named assets_list.csv. When
it runs, the script will list all the files in the current directory with
matching names and prompt you for which one to load.

The script loads the MMS IDs of Portfolios which have been deleted from Alma in the file cancelled.xlsx and identifies them in the URL field.

### Errors

The script contains a hard-coded list of URL patterns that can be flagged as
broken. This can and should be customised for your specific needs.

The script also checks for a file named cancelled.xlsx and uses any value in column A or B to do a partial URL pattern check. cancelled.xlsx is for items which have been cancelled and removed from the catalogue. An example from Alma is included here with the Portfolio MMS ID in Column A and the ISBN from the Parser Parameters in Column B.

Otherwise, for every URL in the list, the script will flag it as broken if:

* the webserver returns a 300 - 399 redirect response AND the URL redirected to is a domain. This test picks up where a deep link to a page or file redirects to the homepage of an organisation
* the webserver returns a "400 Bad Request" response
* the webserver returns a "404 Not Found" response
* the webserver returns a 500 - 599 range server error response.
* the hostname in the URL cannot be resolved (i.e. DNS error)
* a connection timeout occurs (by default this is 90 seconds)
* the connection terminates incorrectly

Other error values (e.g. "401 Not authorised", "403 Forbidden") are _not_ flagged by this script.

### Output

In addition to the CSV menu, the script will output each URL it is checking,
and the result.

Finally, it will produce a CSV file of the broken links, named `broken-links-assets_list.csv`

This report file contains several columns, FILL THIS IN TOMORROW
