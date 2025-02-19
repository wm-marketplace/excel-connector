## Connector  Introduction

Connector is a Java based backend extension for WaveMaker applications. Connectors are built as Java modules & exposes java based SDK to interact with the connector implementation.
Each connector is built for a specific purpose and can be integrated with one of the external services. Connectors are imported & used in the WaveMaker application. Each connector runs on its own container thereby providing the ability to have it’s own version of the third party dependencies.

## Features of Connectors

1. Connector is a java based extension which can be integrated with external services and reused in many Wavemaker applications.
1. Each connector can work as an SDK for an external system.
1. Connectors can be imported once in a WaveMaker application and used many times in the applications by creating multiple instances.
1. Connectors are executed in its own container in the WaveMaker application, as a result there are no dependency version conflict issues between connectors.

## About Excel Connector

## Excel Connector Introduction
Excel connector provides apis to parse the Excel files with tabular structures & returns the table data as java objects. 
The return response can be directly mapped to entity classes that enable it to easily persist the entities into database tables directly.

## Build
You can build this connector using following command
```
mvn clean install
```

## Deploy
You can import connector dist/excel-connector.zip artifact in WaveMaker Application using file upload option.

## Using Excel Connector in WaveMaker Application
## Step 1: Importing the excel-connector to the project
1.Download the latest zip [here](https://github.com/wavemaker/excel-connector/releases)

2.Import the downloaded excel-connector zip into your app using the Import Resource option in the Connector folder.
![image](https://github.com/user-attachments/assets/559480f8-0d28-488d-8a65-dc8b2684cd9d)

![image](https://github.com/user-attachments/assets/ef64b1bb-5aba-4edb-85b1-c0ab2f36f28e)

## step2 : Import a databaseService
1. Import a database into your project.
   <img width="951" alt="image" src="https://github.com/user-attachments/assets/16cd8cb4-5ab8-43ff-a1a6-b51bc1ec32fc" />

## Step 3: Creating Java Service
1.Create a Java Service, named ExcelService

2.Add the following import statements in the Java service created in the above step.

```
import java.util.List;
import java.io.IOException;
import org.springframework.web.multipart.MultipartFile;
import com.wavemaker.connector.excel.ExcelConnector;
import com.wavemaker.marketplacepreviewer.salesdb.service.ChannelsService;
import com.wavemaker.marketplacepreviewer.salesdb.Channels;
```

3.Follow below code snippet for creating a method to insert data to database table from excel file
```
@ExposeToClient
public class ExcelService {
    private static final Logger logger = LoggerFactory.getLogger(ExcelService.class);
	@Autowired
    private ChannelsService channelsService;
    @Autowired
    private SecurityService securityService;
     @Autowired
    private ExcelConnector excelConnector;
    
  public void createUsersFromExcelFile(MultipartFile file) throws IOException {
        List<Channels> channelsList =  excelConnector.readExcelAsObject(file.getInputStream(), Channels.class);
        channelsList.forEach(channel -> {
            channelsService.create(channel);
        });
    }
}
```
## Step 4: Integrating with UI
1.Drag and Drop a FileUpload widget

2.Create a Database CRUD variable for salesDB channel table with name as dbChannelData

3.Drag and Drop a Data Table from the existing Database CRUD variable dbChannelData

4.Create a JavaService variable for the ExcelService created in the previous step with name as ChannelCreationVariable and bind variable parameter to Widgets.fileupload1.selectedFiles[0] and on the onSuccess event of variable give dbChannelData variable

5.For fileUpload widget onSelect event Callback give variable ChannelCreationVariable





