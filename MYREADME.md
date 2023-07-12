## Import into powerbi online

Based on https://learn.microsoft.com/en-us/rest/api/power-bi/imports/post-import



~~~bash
#Install HTTP client, we are going to use httpi
sudo apt-get update
sudo apt install httpie
#Install azure cli
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
#Get powerbi access token
http --form POST https://login.windows.net/testea.onmicrosoft.com/oauth2/token grant_type=password resource=https://analysis.windows.net/powerbi/api username=demo@testea.onmicrosoft.com password=CostM@nagement99! --verbose 
# TODO This is not going to work because I do not have access to the corresponding AAD to create an App Registration which is needed.

# Using azure cli default token does not work either.
az login -u demo@testea.onmicrosoft.com -p CostM@nagement99!
token=$(az account get-access-token --query accessToken -o tsv)
echo $token
http POST https://api.powerbi.com/v1.0/testea.onmicrosoft.com/imports datasetDisplayName==chpinoto.FOCUS.pbix Authorization:"Bearer ${token}" fileUrl=https://github.com/flanakin/cost-management-powerbi/blob/168580715078e3c7f00850eb87487b130e627f0c/FOCUS.pbix


#See all impports
http GET https://api.powerbi.com/v1.0/testea.onmicrosoft.com/imports Authorization:"Bearer ${token}" 
~~~