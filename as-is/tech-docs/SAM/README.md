# SAM Entity Management Web Service Tutorial
## Entity Management Web Service Introduction
The SAM Entity Management web service is a B2B (Business-to-Business) web application that accepts user-customized requests from a client application for SAM information on one or multiple records and then returns the requested data in real time, using the XML format. The transaction is sent using SOAP technology and is encrypted via SSL.  Response times for transactions vary by the number of records and the type of data requested.  The purpose of this documentation is to help with the construction of the client application.

### Building a Client Application
The client application that interacts with SAM must be written by the client's company or organization.  In order to communicate with the SAM server you have to call the web service using SOAP. You will still need to construct the XML request string, but you can call the SAM Entity Management getEntities web method at the web service URL: https://gw.sam.gov/SAMWS/1.0/Entity. Use the XML request string as the single parameter for the web method, which returns the getEntitiesResponse as a string. 

Your client application can then take the getEntitiesResponse and parse it to extract the data.

## Entity Management Request (Client XML)
This is the XML request sent to the SAM server.  The SAM Entity Management getEntities XML is split up into three different sections (authenticationKey, entitySearchCriteria, and requestedData) that are discussed in detail below.

The SAM team suggests using the SmartBear SoapUI tool (http://www.soapui.org/) and the SAM Entity Management WSDL (https://gw.sam.gov/SAMWS/1.0/Entity?wsdl) to create your own sample SAM Entity Management project.  After this, create sample SAM Entity Management requests in SoapUI, point them to the SAM production endpoint (https://gw.sam.gov/SAMWS/1.0/Entity) and review your results.  It may be helpful to review your sample XML along with this documentation to better understand how it works. 

### SAM Entity Management Authentication (authenticationKey)
The first section of the SAM Entity Management XML request is the authenticationKey.  All of this information is mandatory.  This is where you specify the user ID and password that you obtained for the SAM Entity Management XML application. 

``` 
-  <authenticationKey>
      <userID>TESTUSERNAME</userID>
      <password>TESTPASSWORD</password>
   </authenticationKey> 
```

Using the SAM Entity Management web service requires a SAM account, although no specific roles are required to view public data.  Please note that the username and password are encrypted in transit via SSL, but not at rest.



### SAM Entity Management Search Criteria (entitySearchCriteria)
The second section of the SAM Entity Management XML request is the entitySearchCriteria.   This is where you specify the SAM Entity Management records you want returned to your client application, which is done by using the "Core" and "Optional" Identifiers listed below.  You are required to use one and only one type of Core Identifier per transaction.  Use of the Optional Identifiers is not required but can be used to further refine your criteria.  Multiple optional identifiers may be used in a single request.  Multiple records may be returned in any one transaction, though this web service is designed and optimized for single record responses.

*Core Identifiers*

 * DUNSNumber[1]
 
 * CAGECode
 
 * taxpayerIdentificationNumber[2]
 
 *startDate & endDate[3]
 
[1] DUNS Identifiers can include both DUNS and DUNS+4 values combined in the DUNSValue element.
 
[2] taxpayerIdentificationNumber search are not allowed for Public searches.

[3] Both startDate and endDate are required in order for the date to be used as a Core Identifier. Maximum search time allowed between startDate and endDate is 24 hours.

*Optional Identifiers*
* registrationStatus 

* startDate

* endDate

The registrationStatus identifier can be either ‘A’ for Active registrations or ‘E’ for Expired registrations.  If the tag is left empty, SAM returns records found regardless of registration status.  Below is an example of what the entitySearchCriteria section looks like if you are searching for one DUNS number (123412341) and only want to return an Active record.  You would use DUNS as your Core Identifier, and registrationStatus as your Optional Identifier.

```
         <entitySearchCriteria>
            <!--Optional:-->
            <DUNSNumber>123412341</DUNSNumber>
            <!--Optional:-->
            <CAGECode></CAGECode>
            <!--Optional:-->
            <taxpayerIdentificationNumber></taxpayerIdentificationNumber>
            <!--Optional:-->
            <registrationStatus>A</registrationStatus>
            <!--Optional:-->
           <startDate></startDate>  
            <!--Optional:-->
            <endDate></endDate>
         </entitySearchCriteria>
```

SAM Entity Management allows you to search for all SAM entity records that were updated within a certain period.  Below is an example of what the entitySearchCriteria section looks like if you are searching for all the records that have been updated over a 24 hour period, between 11/25/2013 and 11/26/2013.  Remember, 24 hours is the maximum allowable search interval.  In this case, the startDate/endDate interval is your Core Identifier.  Please note the date formats in the example:

```
         <entitySearchCriteria>
            <!--Optional:-->
            <DUNSNumber></DUNSNumber>
            <!--Optional:-->
            <CAGECode></CAGECode>
            <!--Optional:-->
            <taxpayerIdentificationNumber></taxpayerIdentificationNumber>
            <!--Optional:-->
            <registrationStatus></registrationStatus>
            <!--Optional:-->
           <startDate>2013-11-25-00:00:00</startDate>  
            <!--Optional:-->
            <endDate>2013-11-26-00:00:00</endDate>
         </entitySearchCriteria>
```

The <startDate> represents the beginning of the update interval and <endDate> represents the end of the update interval. The format for both <startDate> and <endDate> is YYYY-MM-DD-HH:MM:SS.

### SAM Entity Management Requested Data (requestData)
The third section of SAM Entity Management XML request is the requestData.  All of the information in this section is optional, but at least one of the elements is required in order to be valid.  This is where you specify the groups of information you want returned in the SAM Entity Management getEntitiesResponse XML sent back to you from the SAM server.   Those groups are as follows:
* Core Data (coreData) - Includes, but is not limited to, an entity's general information, TIN or EIN number, financial information, and details about any proceedings in which the entity may currently be involved.

* Assertions (assertions) - Documents self-assertions from each entity. Assertions includes, but is not limited to, data about the types of goods and services the entity provides, the entity size, optional Electronic Data Interchange (EDI), and disaster relief data.

* Representations and Certifications (repsAndCerts) - Documents an entity's representations and certifications related to their small business status, responses to commonly used Federal Acquisition Regulation (FAR) and Defense Federal Acquisition Regulation Supplement (DFARS) provisions/clauses, and Architect-Engineer Responses (SF330 Part II).
Points of Contact (POCs) - The entity will be asked to provide contact information for any mandatory POC based on the information they provided during the registration process. POC types include, but are not limited to, accounts receivable, electronic business, and government business.

You must select at least one group of information to be returned, done so by entering the value 'Y' inside of the element tags.  Below is an example of the requestData section if you wanted to select all 4 groups of information to be returned.

```
         <requestedData>
            <!--Optional:-->
            <coreData>Y</coreData>
            <!--Optional:-->
            <assertions>Y</assertions>
            <!--Optional:-->
            <repsAndCerts>Y</repsAndCerts>
            <!--Optional:-->
            <pointsOfContact>Y</pointsOfContact>
         </requestedData>

```
If there is a particular element you do not wish to view, submit and empty tag for that particular data element.  For example, below is an example of the requestData section if you wanted to select all only Reps and Certs to be returned.

```
         <requestedData>
            <!--Optional:-->
            <coreData></coreData>
            <!--Optional:-->
            <assertions></assertions>
            <!--Optional:-->
            <repsAndCerts>Y</repsAndCerts>
            <!--Optional:-->
            <pointsOfContact></pointsOfContact>
         </requestedData>
```

The schemas included in additional documentation packages indicate which elements are allowed for which access level (Public, FOUO, or Sensitive). If you request data that you do not have authorization for, you will still get a response, but it will not include the restricted elements and you will get a message that indicates that not all requested elements are being returned.

Please be aware of the amount of information you are requesting.  Do not request groups of data you do not need.  Since multiple records can be returned per transaction, the size of the response being returned can get large and take a long time to return. 

## Entity Management Response (Server XML)
This is the XML returned from the SAM server.  The SAM Entity Management getEntitiesResponse XML is split up into two major sections, transactionInformation and listOfEntities.  The transactionInformation section will always be returned.  Depending on the transactionStatus value, found inside the transactionInformation section, the listOfEntities section may or may not be have results.  If there are no results found the listOfEntities will return a null tag.  Review one of the sample Entity Management responses, found in the 'SampleFiles' directory in this documentation package, along with this documentation to better understand how it works. 

### SAM Entity Management Response Transaction (transactionInformation)
The first section of the getEntitiesResponse XML is transactionInformation.  All of the elements in this section are mandatory.  This is the first place you will check in your parser to see the status of the transaction by looking at the value of the transactionStatus element.  Below is a key for that element.

_transactionStatus Key_

The following two transactionStatus codes and messages are considered successful responses.  You will see these responses if you have a sent a proper getEntities XML request and received either a response with 1 or more entities (code 01) or no entities (code 02).

* 01 = <  Null Tag >
* 02 = No records found for the specified criteria.

The following transactionStatus codes are error codes that can occur for authentication, search criteria, or database error.

* 11 = Error: Authentication failure. Please check your username and password.
* 21 = Error: Invalid search criteria. At least one search criteria element is required.
* 22 = Error: Invalid search criteria. When performing a search by date, both start date and end date are required.
* 23 = Error: Invalid search criteria. The correct date format is YYYY-MM-DD-HH:MM:SS
* 24 = Error: Invalid search criteria. The time period cannot exceed 24 hours.
* 31 = Error: Error while retrieving data. Please wait and try again. If this issue persists, please contact the Federal 
Service Desk and provide your username, endpoint URL, exact date/time, and error code “31”.

The other three elements, transactionMessage, dataRetrievalTime, and numberOfRecordsReturned are informational elements.  The transactionMessage element will give a more detailed description of any error that occurs in the transaction, similar to the list above. It may also be used as a message board for system information. The dataRetrievalTime element will display the number of seconds it took the SAM server to process the request and return the getEntitiesResponse XML.  The numberOfRecordsReturned element will display the number of unique entities your search criteria returned.  Below is an example of what the transactionInformation section should look like if there are no problems.

```
            <transactionInformation>
               <transactionStatus>01</transactionStatus>
               <transactionMessage/>
               <dataRetrievalTime>35.2</dataRetrievalTime>
               <numberOfRecordsReturned>326</numberOfRecordsReturned>
            </transactionInformation>
```

This sample transaction was a successful transaction that took 35.2 seconds to complete and returned 326 records in the response.

### SAM Entity Management Response Results (listOfEntities)
The second section of the getEntitiesResponse XML is the listOfEntities. This section may be null if there are no entities that match the search criteria or contain a list of one or more unique records. This is where all of the SAM record information is returned. Because of the vast amount of data in this section only the first two layers of the SearchResult section are displayed below as an example. 

```
            <listOfEntities>
               <entity>
                  <entityIdentification>
                     <DUNS>1233456789<DUNS>
                     <DUNSPlus4/>
                     <CAGECode>0KXX9</CAGECode>
                     <NCAGECode/>
                     <DODAAC/>
                     <legalBusinessName>ENTITY NAME LEGAL</legalBusinessName>
                     <DBAName/>
                     <registrationPurpose>Z2</registrationPurpose>
                     <registrationStatus>A</registrationStatus>
                     <registrationDate>2013-07-10 00:00:00</registrationDate>
                     <lastUpdateDate>2013-07-10 00:00:00</lastUpdateDate>
                     <expirationDate>2014-10-21</expirationDate>
                     <activationDate>2013-10-21</activationDate>
                     <noPublicDisplay/>
                     <exclusionStatus/>
                  </entityIdentification>
        	      + <coreData>
       	      + <assertions>
                  + <repsAndCerts>
                  + <pointsOfContact>
              </entity>
         </listOfEntities>
```

The main element in the listOfEntities section is the entity element.  For every SAM record returned there will be one instance of the entity element which encapsulates all of that record's data.  Inside the entity element there are five main sub-sections:
* entityIdentification
* coreData
* assertions 
* repsAndCerts
* pointsOfContact

The entityIdentification section will always be returned for all valid entities, regardless of the request criteria specified.  This section contains core identification data such as DUNS and Legal Business Name.  The final four sections may or may not be displayed depending on the initial getEntities request.  Under each of these sections are another layer of sub-sections and then the data elements themselves.  Once again, if you request data that you do not have authorization for, you will still get a response, but it will not include the restricted elements.  For example, if you are a FOUO user and request coreData, you will not see financial information as that requires the Sensitive Entity Management Data Viewer privileges.

If you would like to see the complete layout of a SAM Entity Management getEntitiesResponse, take a look at the file ‘EntityManagementXMLResponse-A.xml' file found in the SampleFiles folder.

## How to request a large set of records from SAM
If you are trying to keep a database updated with daily changes, the SAM Entity Management Extracts are a better option. We do not recommend using SAM Entity Management XML to keep your database up to date because of the demands on our web servers and the likelihood that you will not always be able to get all changes at once. 

The only method of retrieving multiple records is to use the <startDate> and <endDate> elements to retrieve the most current SAM information for all records that have had their profile record added/updated/expired/deleted during the specified time period.  You are allowed a maximum interval of 24 hours between the <startDate> and <endDate> elements in each transaction, which means you are allowed to retrieve up to a day's worth of SAM record changes in one transaction.  Since all SAM registrants must update their profile at least once a year in order to remain active, you won't find any active CCR registrations with a <startDate> that is older than 365 days.  You can specify that only certain SAM registrations statuses are returned by using the <registrationStatus> element in the same transaction.

Before you start developing an application that will request a large set of records from SAM, please remember the following.  This is a very resource intensive process which needs to have the SAM Entity Management getEntities XML as specific and optimized as possible.  Only request the information you really need in the <requestedData> section, and if you are only looking for active SAM records, then specify this in the <registrationStatus> element. 
If you would like to see an example layout for requesting a large set of records from CCRXML then take a look at the files 'EntityManagementXMLResponse-H.xml' and 'EntityManagementXMLResponse-I.xml' found in the SampleFiles folder and also available by clicking on the links located in the "Sample Entity Management Request and Response Files" section of this tutorial.

## Sample Entity Management Request and Response Files
Take a look at the XML files referenced below for examples.  Below each pair of XML files is a brief explanation on each sample. 

* Searches for a single entity by DUNS and requests all four optional elements (Core Data, Assertions, Reps and Certs, and POCs). [sample]()

* Searches for a single entity by DUNS and requests only Core Data. [sample]()


* Searches for a single entity by DUNS and requests only Assertions. [sample]()


* Searches for a single DUNS and requests only Reps and Certs. [sample]()


* Searches for a single entity by DUNS and requests only POCs. [sample]()


* Searches for a single entity by Taxpayer Identification Number and requests only Core Data.[sample]()


* Searches for a single entity by CAGE code and requests all four optional elements (Core Data, Assertions, Reps and Certs, and POCs). [sample]()

* Searches for all SAM records updated between 2013-11-25-00:00:00 and 2013-11-26-00:00:00 and requests only Core Data. [sample]()

* Searches for all SAM records expired between 2013-11-25-00:00:00 and 2013-11-26-00:00:00 and requests only Core Data. [sample]()
