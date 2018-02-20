;~  https://www.yelp.com/developers/documentation/v3/authentication
ID:="m1qHe3kz8R0V-Kcgz2fz1w"
Secret:="BxSgm4B4Xq1m129gSAgK8rAqxsJRYh3hbdGzaAhC11aqMt4MUpHMtXETUG6DHQj9"
 
EndPoint:="https://api.yelp.com/oauth2/token"
QueryString:=QueryString_Builder({"grant_type":"client_credentials","client_id":ID,"client_secret":Secret})
;***********API call to get Authorization Token*******************
HTTP := ComObjCreate("WinHttp.WinHttpRequest.5.1")
HTTP.Open("POST",EndPoint . QueryString)
HTTP.Send()
data:=HTTP.ResponseText ;~ MsgBox,,title, % response
RegExMatch(data,".*access_token\x22:\s\x22(?<Token>.*?)\x22.*",Yelp_) ;RegEx to grab token
;~  MsgBox % Yelp_token
 
;~  https://www.yelp.com/developers/v3/manage_app?app_created=True
;~  https://www.yelp.com/developers/documentation/v3/business_search
EndPoint:="https://api.yelp.com/v3/businesses/search"
 
;~  https://www.yelp.com/developers/documentation/v2/all_category_list
;~  https://www.yelp.com/developers/documentation/v2/all_category_list/categories.json
QueryString:=QueryString_Builder({"term":"restaurants","location":"Coppell, TX","categories":"chicken_wings"})
 
;***********API call to yelp with Credentials*******************
HTTP := ComObjCreate("WinHttp.WinHttpRequest.5.1")
HTTP.Open("GET", EndPoint . queryString)
HTTP.SetRequestHeader("Authorization","Bearer " . Yelp_token)
HTTP.Send()
Response_Data:=HTTP.ResponseText ;~ MsgBox,,title, % response
Response_Data:=formatjson(Response_Data)
SciTE_Output(Response_Data) ;Text,Clear=1,LineBreak=1,Exit=0
 
 
;***********QueryString Builders*******************
QueryString_Builder(kvp){
for key, value in kvp
  queryString.=((A_Index="1")?(url "?"):("&")) key "=" value
return queryString
}
