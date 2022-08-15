#Loopback

npm i -g @loopback/cli
lb4 app

### DataSource ms sql

lb4 datasource 
Name : hms
Connector : Microsoft SQL
Connection String url --> Skip
host: 192.168.42.180
port: 1433
user: user ที่ใช้เข้า database
password: pass ที่ใช้เข้า database
database: HMSDB

### HIS Service connecttor
lb4 service
Service type: Local service class bound to application context
Service name: HmsConnector

Edit file
```
import {HmsDataSource} from '../datasources';
import inject @loopback/core
```

 Add @inject to inject parameters in constructor
 ```
@inject('datasources.hms') (name:hms)
private hmsDataSource: HmsDataSource,
```

```
  async query<T>(sql: string, params: Array<string> = []) {
    // console.log(sql);
    return new Promise<T>( (resolve, reject) => {
      // eslint-disable-next-line @typescript-eslint/no-floating-promises
      this.hmsDataSource.connector?.execute?.(sql, params, (error: object, result: T) => {
        if (error) return reject(error);
        resolve(result)
      });
    });
  }
```

### HIS Service
lb4 service
Service type: Local service class bound to application context
Service name: HmsService
```
import {HmsConnectorService} from './hms-connector.service';
```

 Add @inject to inject parameters in constructor
 ```
@service(HmsConnectorService)
    public hmsConnectorService: HmsConnectorService,
```


Add function sql query use this.hmsConnectorService.query<Array<object>>(sql);

Add getPatient
 ```
  async getPatientByHn(hn: string): Promise<Array<object>> {
    const sql =SELECT * FROM patient WHERE hn=${hn}`;
    return this.hmsConnectorService.query<Array<object>>(sql);
  }
 ```
 

 ### Lb4 controller
lb4 controller
Controller class name: Hms
Empty Controller

 ```
import {HmsService} from '../services/hms-service.service';
```
 Add constructor
 ```
@service(HmsService) public hmsService: HmsService,
```
Add function get
@get('/patient/{hn}')
  async getPatient(
    @param.path.string('hn') hn: string,
  ): Promise<Array<object>> {
    const data = await this.hmsService.getPatient(hn);
    return data;
  }

### DataSource mongodb
lb4 datasource 
Name : mongo
Datasource name: mongo
connector mongo:  MongoDB
Connection String url --> skip
host: 10.1.1.98
port: 27018 (default port 27017)
user: ที่ใช้เข้า database mongo
password: ที่ใช้เข้า database mongo
database: sakon
Feature supported by MongoDB v3.1.0 and above: (Y/n) Y (db.version())

***authSource: 'admin',
 
### Model
Lb4 model
Entity model

### Repository
lb4 repository
Select the datasource MongoDatasource
Select model

### Controller
Lb4 controller
CRUD 




### ENV
Create file .env in root path
environment=development

npm install --s dotenv

import * as dotenv from 'dotenv';
dotenv.config();

process.env.environment === "development" ? "127.0.0.1" : "192.168.42.180"

