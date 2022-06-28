# Recommendations UI



## Settings



### Search



### Columns



### Filter expression
Write an expression that returns a boolean result.  
`True` = not filtered `False` = filtered.
example.  
`price` > `100`
this returns `True` for all products with a price larger than 100 thus removing all items with a price lower than 100.



### Boost expression




### InteractionFilter




### Trend
Trend limits the time intervall wich the recomendations are based on. `Max trend` uses a short timeintervall resulting in the ML model only recomends products that are trending last few weeks.



### Limit
Limit decides how many recomended items are returned. limit 4 = 4 recommended items. Limit 12 = 12 recommended items.



### Max orders
Number of orders that are taken into concideration when recommending products. `Max orders = 1` means only the items included in the latest order are used as purchase history for that user. `Max orders = 2` means the items in the two latest orders are taken into account.



### Max interactions
Number of items that are taken into concideration when recommending products. `Max items = 1` means only the latest purchased item is used as purchase history for that user. `Max items = 2` means the two latest items are taken into account.



### Allow items from history
sets a filter so that all items the user have bought cannot be recommended. 



### shuffled
If Limit is set to 10 the items are recomended in order (most likly next purchase is at the top).  
by enabling `shuffled` it still recommends the top 10 items but the order of the top 10 items are shuffled. 



### Advanced
Name: is the variable name.
Field: the field wich you are taking data from
Option: "all" means all items in the users purchase history. "items" means all items sent in through the API by the customer. When the customer makes an API call for the recommendation they can include items in the API call. for example items in the basket, the item you are currently looking at etc.
Example below where `boughtArticleGroup` returns a comma seperated string `'item1,item2,item3,item4'`.
to convert this into a filter
```
hasany(split(basketArticleGroup,','),makearray('Thin Case'))
```
example of advanced query.  
```
{"Context":[{"Name":"boughtArticleGroup","Field":"prod_collection_name","Option":"all"}, {"Name":"basketArticleGroup","Field":"prod_collection_name","Option":"items"}]}
```
**Examples**  
*Advanced:*
```
{"Context":[{"Name":"bought_phone_model","Field":"phone_model","Option":"all"}]}
```
*Filter expression:*
```
hasAny(split(bought_phone_model,","),makeArray(phone_model))
```
`split(bought_phone_model,",")` returns an array of your purchase history ("Field":"phone_model"): ['bought_phone_model_1','bought_phone_model_2','bought_phone_model_3','bought_phone_model_4']  

`makeArray(phone_model)` returns an array of all `phone_model`: ['phone_model_1','phone_model_2','phone_model_3','phone_model_n']  

`hasAny` checks if elements in `array 1` is contained in `array 2`  

in this case the customers purchase history includes `iPhone 12/Pro MagSafe` and `iPhone 12/Pro`
The expression `hasAny(split(bought_phone_model,","),makeArray(phone_model))` will thus return `true` for all pruducts where `phone_model` = `iPhone 12/Pro MagSafe` or `iPhone 12/Pro` and `false` for all other products. Thus only products with the same `phone_model` will be recommended.

If you change the expression to `hasAny(split(bought_phone_model,","),makeArray(phone_model)) = FALSE` everything will be inverted thus only recomending products where the `phone_model` **!=** `iPhone 12/Pro MagSafe` or `iPhone 12/Pro`.

![image](https://user-images.githubusercontent.com/102239423/171135517-3d3eaeeb-7785-460e-a242-2a6e3cfaceb4.png)
![image](https://user-images.githubusercontent.com/102239423/171135706-fcc4ad7f-7066-441a-8e4d-c4162cacebec.png)

[NOTE! when adding an item to in the `Search for items` you **DO NOT** add this item to your purchase history, these items are sent in from the API meaning they will only be affected by a filter expression if you use the option `item` instead of `all`]  

See example:
![image](https://user-images.githubusercontent.com/102239423/171145795-877fb7b8-6e02-4bf0-857b-985deafe6efd.png)



### API



### Add field limit 
Field limit lets you set a filter on how many of each category should be recommended.  

This is usually used on product category where you only want to include ex. max 2 of each product category.



### Save as new profile
Saves the current configuration as a profile new



### Save profile
Overrides the current profile with the current configuration.



### Delete profile
Delets selected profile.



## Front page



### Search for items
Items added in the search items window are counted as items given to the platform through the API call. This means they dont not affected by `boughtArticleGroup` but affects `basketArticleGroup`. 



### Profile
saved profiles



### Refresh




### Clear user and items
removes selected items and user



### Bench
Shows how fast the recommendation are returend when making an API call. (if your datamodel has MLFilter active for many fields the recommendations will be slower).



### Add random item
same as **search for items** but adds a random item.



### Add random user
Selects a random user and shows what items this user has previously bought and the returned recommended items based on this. 



### Recommendation output
The output is what the API returns to the customer.  
All fields active in the **Columns** section are returnerd. if the `image` is active the output will only show the image and format. by disabling the image you can see all other fields that are returned.  
With image active:
![image](https://user-images.githubusercontent.com/102239423/170311295-0a3f1097-cc4f-4829-a4fb-8962b3b5d5e7.png)
With image inactive:
![image](https://user-images.githubusercontent.com/102239423/170311416-cd596ff0-02d8-44b8-91d9-483ddcccb075.png)



# Data available in recommendations



## ml meta
To show an image and name on your recomendations you have to choose a format field and an image field under items AND activate the ml meta for these fields.



## ml filter
To write filter expressions based on fields you have to activate ml filter for these fields



# Recommendations in emails



## Introduction
We can supply personalized recommendations in automated email flows. The technical integration varies between partners, but the main difference compared to segments is that we supply an individually customized set of products per user.



## Partners
Here we specify how the process works between us and specified partners



### Voyado
Voyado has an ftp server to which we push a file of the form
```
ContactId,Skus,ExpiryDate
00000000-0000-0000-0000-000000000000,"Item692,Item165,Item835,Item166,Item836,Item838,Item277,Item504,Item332,Item218,Item608,Item528",9999-12-31T00:00:00.000+0000
0001394c-9e71-43d6-86f3-ada901fc4c10,"Item218,Item411,Item135,Item504,Item692,Item202,Item1035,Item835,Item412,Item277,Item610,Item515",9999-12-31T00:00:00.000+0000
00021b19-db3d-4d26-84ce-ad56g0f028e3,"Item165,Item702,Item701,Item146,Item166,Item835,Item1035,Item610,Item836,Item135,Item218,Item150",9999-12-31T00:00:00.000+0000
```
The list of `Skus` is generated from our recommendation engine for the user specified under `ContactId`, where the 0 line is the fallback recommendation.

-- Insert how to setup Voyado Export here --

Once the recommendation flow is set up and an initial export has been sent, the recommendations can be previewed in Voyado. This is done by us selecting a few contactIds from the platform for them to check. Then the customer triggers a support ticket by sending an email to support@revide.se (revide is the old name, this may be updated) with the contactIds and requests checking the preview.

**Note:** Historically there has been some issues due to contacts being labeled as "Contact" instead of "Member". This should be resolved as of April 2022, but to be safe, it could be a good idea to locate and include a user labeled "Contact" that has a purchase history. 



# Recommendation profiles



## Infobaleen rekommendationsbibliotek
2022-03-24

Grundinställningar:  
Produkter i lager  
Vid tydliga könsrelaterade produkter: Se om man bara ska rekommendera produtker med samma gender  
…  
___



### Olika rekommendationer



#### Standard rekommendation: Standard
Rekommendera det mest sannolika nästa köpet (medium trend)  
**Syfte:** Hög sannolik att konvertera  
**Expression:**  
**Trend:** 



#### Liknande produkter rekommendation: Similar
Rekommendera produkter ur samma kategori  
**Syfte:** Hög igenkänningsfaktor  
**Expression:**  
**Trend:** 



#### Inspiration rekommendation: Inspiration
Rekommendera produkter inom kategorier kunden ej handlat inom tidigare / vald produkt  
**Syfte:** Inspirera till köp i de mest relevanta kategorier man ännu ej handlat inom / produkten ej är i  
**Expression:**  
**Trend:** 



#### Win-back rekommendation: Win-back
Rekommendera mest sannolika produkter en kund kommer köpa utifrån historiskt köpbeteende  
**Syfte:** Presenter de produkter en churnad kund har högst sannolikhet att köpa  
**Expression:**  
**Trend:** 



#### Check-out rekommendation: Check-out 
Rekommendera billigare produkter utifrån vad kunden lagt i sin varukorg  
**Syfte:** Öka AOV med en relevant produkt i det lägre prissegmentet  
**Expression:** `tofloat(Price) < tofloat(last(basketArticlePrice))*0.8`
**Trend:**



#### Tack för senast order rekommendation: Last purchase 
Rekommendera mest sannolika produkter utifrån kundens senaste order  
**Syfte:** Öka andelen kunder som gör ett nästa köp  
**Expression:**  
**Trend:** 



#### Produkter på rea rekommendationer: On sale
Rekommendera produkter som är på rea  
**Syfte:** Öka rea fsg genom att visa relevanta produkter på rea.  
**Expression:**  
**Trend:** 

