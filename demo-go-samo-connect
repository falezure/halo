package main

import (
	"fmt"
	"net/http"
	"net/url"
	"time"
	"io/ioutil"
	"strings"
	"strconv"
	"log"
)

func SamoRequst() (string, error) {
    client := &http.Client{Timeout: 120 * time.Second}

    // pls modify the target URL     
    url_samo , err := url.Parse("http://***/xmlapi/invoke")
    if err != nil {
        log.Fatal(err)
        return "error", err
    }
    
	// pls modify the target request SOAP
    reqBody := `<?xml version="1.0" encoding="UTF-8"?> 
<SOAP:Envelope xmlns:SOAP="http://schemas.xmlsoap.org/soap/envelope/"> 
   <SOAP:Header> 
	  <header xmlns="xmlapi_1.0"> 
		 <security>
			<user>***</user>
			<password hashed="false">***</password>
		 </security>
		 <requestID>XML_API_client@25</requestID>
	  </header>
   </SOAP:Header>
   <SOAP:Body>
   <find xmlns="xmlapi_1.0">
	  <fullClassName>netw.NetworkElement</fullClassName>
	  <filter classname='netw.NetworkElement'>
		  <or>
			<equal name="siteId" value="192.168.5.37"/>	
			<equal name="siteId" value="192.168.5.36"/>	
			<equal name="siteId" value="192.168.5.35"/>	
			<equal name="siteId" value="192.168.5.38"/>	
		  </or>
	  </filter>    
	  <resultFilter>
		  <attribute>sysDescription</attribute>
		  <attribute>outOfBandAddress</attribute>
		  <attribute>inBandSystemAddress</attribute>
		  <attribute>sysUpTime</attribute>
		  <attribute>sysUpTimeResyncTimestamp</attribute>
		  <attribute>chassisType</attribute>
		  <attribute>version</attribute>
		  <attribute>olcState</attribute>
		  <attribute>siteId</attribute>
		  <attribute>siteName</attribute>
		  <children recursive="no" />
	  </resultFilter>
   </find>
   </SOAP:Body>
</SOAP:Envelope>`
    fmt.Println(reqBody)
    req, err := http.NewRequest("POST", url_samo.String(), strings.NewReader(reqBody))
    if err != nil {
    	log.Fatal(err)
	    return "error", err
    }
	req.Header.Add("Content-type", "text/xml")
	req.Header.Add("charset", "UTF-8")
	req.Header.Add("Content-length", strconv.Itoa(len(reqBody)))

	resp, err := client.Do(req)
	if err != nil {
		log.Fatal(err)
		return "error", err
	}

	if resp.StatusCode != 200 {
		println("Error:", resp.Status)
	}

	defer resp.Body.Close()
	respBody, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		return "error", err
	}
	
	return string(respBody),nil

}


func main() {
	fmt.Println("the request is:")
	respon, err := SamoRequst()
	if err != nil {
		log.Fatal(err)
	    return 
	}
	fmt.Println("the result is:")	
	fmt.Println(respon)
}
