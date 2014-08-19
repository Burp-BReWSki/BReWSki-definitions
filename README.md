BReWSki-definitions
===================
This repository contains definitions that have been developed for the BReWSki plugin for BurpSuite. Example definitions can be found in the "**TestDefinitions**" folder.

##Defintion Details
When creating a defintion the following global variables must be set:

    var name = "Empty Definition"; //Name of scan check
    var description = "For example purposes only";//this describes the check
    var detail = ""; //this is the finding's actual detail when it's found by the scanner
    var type = "Passive"; //Passive or Active
    var severity = "High"; // Expected values are "High", "Medium", "Low", "Information" or "False positive".
    var background = ""; //Describes the underlying issue
    var remediation = ""; //Describes remedation
    var confidence = "Certain";  // "Certain", "Firm" or "Tentative".
    var output = "";
    var requestMarkers = [0,0];
    var responseMarkers = [0,0];
    var result = false; // "true" or "false"

All definitions must return "true" or "false" to indicate if the definition successfully discovered an issue. This return value must be stored in the "result" variable.

The following global variables are injected into the javascript engine and are available within your definition:

    request; //This is a string representation of the original request - active checks only
    response; //This is a string representation of the server response
    url; //This is a string represenation of the full URL

Additional global variables can be added as needed. If you find something that you think should be included in the definition namespace create an Issue and we can take a look at adding it.

##Accessing Java
The Java runtime environment is available from your definition and can be used to implement functionality that may not be supported by javascript or in cases where it is easier to leverage some internal functionality within Java. For example, the following snippet from an example definition shows the ability to open a raw socket from your scan definition:

    function test(){
        try{
            importPackage(java.net);
            importPackage(java.io);
            var target = url.substring(url.indexOf("://")+3,url.indexOf(":",url.indexOf("://")+3));
            var sock = new Socket(target,23);
    
            if(sock.isConnected() == true){
                var br = new BufferedReader(new InputStreamReader(sock.getInputStream()));
                output += String(br.readLine()) + " <br>";
                output += String(br.readLine()) + " <br>";
                output += String(br.readLine()) + " <br>";
                output += String(br.readLine()) + " <br>";
                detail += "The server responded with the following: <br>" + output;
    
                sock.close();
                return true;
            }
    
            return false;
        }

Documentation for scripting Java from within rhino can be found on the Mozilla project page: https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Rhino/Scripting_Java
