# Label API - Domestic (US Only)

**Production URI**: https://www.wearerichmom.com

**Endpoint**: /api/v1/dhlec/label

## Create a Label

**Method**: POST

**Headers:**
    
    Content-Type: application/json
    X-API-Key: CLIENT_API_KEY
	
**Body**: JSON

    {
        "orderId": "test_order_01",
        "sellingChannel": "eBay",
        "totalWeight": 200,
        "consigneeAddress": {
            "name": "John Doe",
            "address1": "123 S 66th st",
            "address2": "",
            "city": "Philadelphia",
            "state": "PA",
            "country": "US",
            "postalCode": "19142",
            "email": "user@email.com",
            "phone": "123-456–7890"
        },
        packageContent: {
            "contentDetail": [
                "description": "Shirt",
                "quantity": 1,
                "price": 10,
                "hsCode": "61091000",
                "origin": "TH",
            ],
            "currency": "USD",
            "category": "G"
        },
        "test": true
    }

**Body Description**:
    
    orderId: Selling Platform Order ID
        Requirement: Mandatory
        Data Type: String
        Valid Data: Any
        Max Length: 20 
        
    sellingChannel: Selling Platform Name
        Requirement: Mandatory
        Data Type: String
        Valid Data: eBay|Amazon|Etsy|Other
    
    totalWeight: Package Weight in grams (g)
        Requirement: Mandatory
        Data Type: Integer or Float
        Valid Data: Any Numeral 
        Example: e.g. 200, 120.50, etc
    
    consigneeAddress.name:
        Requirement: Mandatory
        Data Type: String
        Valid Data: Any

    consigneeAddress.address1:
        Requirement: Mandatory
        Data Type: String
        Valid Data: Any

    consigneeAddress.address2:
        Requirement: Optional
        Data Type: String
        Valid Data: Any or Blank, or Omit

    consigneeAddress.city:
        Requirement: Mandatory
        Data Type: String
        Valid Data: Any

    consigneeAddress.state:
        Requirement: Mandatory
        Data Type: String
        Valid Data: Two-Letter State Abbreviations
        Max Length: 2
        Example: NY

    consigneeAddress.country:
        Requirement: Mandatory
        Data Type: String
        Valid Data: Two-Letter Country Code, US Only
        Max Length: 2
        Default: US

    consigneeAddress.postalCode:
        Requirement: Mandatory
        Data Type: String
        Valid Data: 5-digit or 9-digit US and Territory Zipcode Only
        Max Length: 10
        Example: 55416 or 55416-4625

    consigneeAddress.email:
        Requirement: Mandatory
        Data Type: String
        Valid Data: Any

    consigneeAddress.phone:
        Requirement: Mandatory
        Data Type: String
        Valid Data: Any
    
    packageContent.contentDetail:
        Requirement: Mandatory
        Data Type: Array
        Min Item in Array: 1
        Max Items in Array: 4

    packageContent.contentDetail.description:
        Requirement: Mandatory
        Data Type: String
        Max Length: 30
        Valid Data: Any

    packageContent.contentDetail.quantity:
        Requirement: Mandatory
        Data Type: Integer
        Valid Data: Any Integer

    packageContent.contentDetail.price:
        Requirement: Mandatory
        Data Type: Integer or Float

    packageContent.contentDetail.hsCode:
        Requirement: Optinal but Recommended
        Data Type: String
        Valid Data: Valid HS Code

    packageContent.contentDetail.origin:
        Requirement: Optional but Recommended
        Data Type: String
        Valid Data: Two-Letter Country Code, US Only
        Max Length: 2
        Default: US

    packageContent.currency:
        Requirement: Mandatory
        Data Type: String
        Valid Data: Three-Letter Currency Code, US Only
        Max Length: 3
        Default: USD

    packageContent.category:
        Requirement: Mandatory
        Data Type: String
        Valid Data: G|D|S|R|M|O
        Definition:
            G: Gift
            D: Document
            S: Commercial Sample
            R: Returned Goods
            M: Merchandise
            O: Other

    test: use this to add 'TEST' as a prefix in packageId, 
        so it's not included when manifesting the packages
        Requirement: Optional
        Data Type: Boolean
        Valid Data: true or false
        

**Example Response**: 200 OK

    {
        "createdOn": "2022-02-14T04:21:55.000Z",
        "dhlPackageId": "4231402267890349",
        "encodeType": "BASE64",
        "format": "PNG",
        "labelData": "iVBORw0KGgoAAAANSUh...",  # Base64 Label Data
        "labelDetail": {
            "customsDetailsProvided": false, 
            "inboundSortCode": "G67", 
            "intendedReceivingFacility": "EWR", 
            "mailBanner": "PS LIGHTWEIGHT", 
            "outboundSortCode": "72", 
            "serviceLevel": "GRD", 
            "sortingSetupVersion": "1"
        }, 
        "packageId": "TEST-EHEH-EWR-14022022-112152", 
        "trackingId": "420191429374869903508237399523"
    }

**Example Response**: Bad Request

    {
        "type": "https://api-sandbox.dhlecs.com/docs/errors/400.0004004",
        "title": "Dynamic Validation Failed",
        "invalidParams": [
            {
              "name": "name",
              "path": "/package/consigneeAddress/name",
              "reason": "One of Name or Company Name is a Requirement field"
            },
            {
              "name": "companyName",
              "path": "/package/consigneeAddress/companyName",
              "reason": "One of Name or Company Name is a Requirement field"
            }
        ]
    }

## Get an Existing Label
**Method**: GET

**Headers:**
    
    Content-Type: application/json
    X-API-Key: CLIENT_API_KEY

**Params**

    packageId       # provided when creating a Label
    dhlPackageId    # provided when creating a Label

**Example Response**: 200 OK

    {
        "createdOn": "2022-02-14T04:21:55.000Z",
        "dhlPackageId": "4231402267890349",
        "encodeType": "BASE64",
        "format": "PNG",
        "labelData": "iVBORw0KGgoAAAANSUh...",  <- Base64 Label Data
        "labelDetail": {
            "customsDetailsProvided": false, 
            "inboundSortCode": "G67", 
            "intendedReceivingFacility": "EWR", 
            "mailBanner": "PS LIGHTWEIGHT", 
            "outboundSortCode": "72", 
            "serviceLevel": "GRD", 
            "sortingSetupVersion": "1"
        }, 
        "packageId": "TEST-EHEH-EWR-14022022-112152", 
        "trackingId": "420191429374869903508237399523"
    }

**Example Response**: 200 Failure - Invalid packageId or dhlPackageId

    {
        "endpoint": "/api/sandbox/v1/dhlec/label",
        "message": "Invalid packageId or dhlPackageId",
        "status": "Failure"
    }

**Example Response**: 200 OK Failure - packageId or dhlPackageId Missing

    {
        "endpoint": "/api/sandbox/v1/dhlec/label",
        "message": "packageId or dhlPackageId Missing",
        "status": "Failure"
    }
