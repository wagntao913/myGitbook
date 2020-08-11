# [Nest(NestJS) -- A progressive Node.js framework](https://docs.nestjs.com/openapi/introduction)

### Nest 安装 与项目创建
  - **环境要求**    下载安装 Node.js、npm包管理器 
  - **Nest安装**    ``` npm install -g @nestjs/cli ```
  - **检查安装**    ``` nest -v ```
  - **创建项目**    ``` nest new <project-name> ```  

  <img src="https://person-study.oss-cn-beijing.aliyuncs.com/create-nest-work.png">

  - **启动项目**    ``` npm run start ```
  > 使用 ``` nest start -w ``` 启动项目后，当文件被保存自动更新项目  

  <img src="https://person-study.oss-cn-beijing.aliyuncs.com/nest-start-successfully.png">
  
  - **项目目录**
  ```
    ├─ src                                   
    │   │── app.controller.spec.ts    
    │   │── app.controller.ts         # 带有单个路由的基本控制器
    │   │── app.module.ts             # 应用程序的根模块
    │   │── app.service.ts            # 处理具体业务               
    │   └── main.ts                   # 应用程序入口文件
  ```    
  - **创建模块**  
    ``` nest g [文件类型] [文件名] [文件目录（src目录下）] ```
    1. 创建 service : ``` nest g service users users```
    2. 创建 module : ``` nest g module users users```
    3. 创建 controller : ``` nest g controller users users```

### 项目优化
  - **集成SwaggerUI**
    1. 安装依赖 ``` npm install --save @nestjs/swagger swagger-ui-express ```
    2. 项目中使用 **main.ts**
        ``` 
        import { NestFactory } from '@nestjs/core';
        import { AppModule } from './app.module';
        import { SwaggerModule, DocumentBuilder  } from '@nestjs/swagger'   // 引入swagger插件
        
        sync function bootstrap() {
          const app = await NestFactory.create(AppModule);
          // DocumentBuilder是一个辅助类，有助于结构的基本文件SwaggerModule。它包含几种方法，可用于设置诸如标题，描述，版本等属性
          const options = new DocumentBuilder() 
            .setTitle('my-nest-project-api-docs') // api文档标题
            .setDescription('The cats API description') // api文档描述
            .setVersion('1.0') // api文档版本
            .build();
          // 为了创建完整的文档（具有定义的HTTP路由），我们使用类的createDocument()方法SwaggerModule。此方法带有两个参数，分别是应用程序实例和基本Swagger选项。
          const document = SwaggerModule.createDocument(app, options);
          最后一步是setup()。它依次接受（1）装入Swagger的路径，（2）应用程序实例, （3）描述Nest应用程序的文档。
          SwaggerModule.setup('/api-docs', app, document);
          await app.listen(3000);
          console.log('start successfully, run http://localhost:3000/api-docs/')
        }
        ```
      3. 启动项目，打开 http://localhost:3000/api-docs/, swaggerUI 集成成功,具体设置后文会深入讲解
      <img src="https://person-study.oss-cn-beijing.aliyuncs.com/SwaggerUI.png">
      
  - **连接MongDB** 
    1. 创建库lib并安装依赖 
      创建一个库lib ``` nest g lib db ```,设置名称为@libs
      <img src="https://person-study.oss-cn-beijing.aliyuncs.com/createlib.png">
      安装针对ts的数据库typegoose
      ``` npm add nestjs-typegoose @typegoose/typegoose ```   
      所以安装 mongoose
      ``` npm add mongoose @types/mongoose ```   
      添加模块的 crud，crud这个接口可以实现增删查改
      ``` npm add nestjs-mongoose-crud ```
      生成lib/db目录结构  
      
        ```
          ├─ src                                   
          │   │── db.module.ts             
          │   │── db.service.ts                
          │   │── db.service.spec.ts                
          │   └── index.ts                 
        ```  
       
    2. 连接和配置数据库
      在libs/db/src/db.module.ts中，设置数据库连接与部分配置
        ```
        import { DbService } from './db.service';
        import { TypegooseModule } from 'nestjs-typegoose';

        @Module({
          imports:[
            TypegooseModule.forRoot('mongodb://localhost:27017/nest',{
              useNewUrlParser: true,
              useUnifiedTopology: true,
              useCreateIndex: true,
              useFindAndModify: false,
            })
          ],
          providers: [DbService],
          exports: [DbService],
        })
        ```
        在src/app.module.ts 中引入数据库模块
        ```
          import { DbModule } from '@libs/db';
          @Module({
            imports: [
              DbModule,
              UsersModule,
            ],
            controllers: [AppController, UsersController],
            providers: [AppService],
          })
        ```
    3. 创建一个用户模块
      在libs/db/src目录下，创建 models 文件夹, 创建 user.model.ts 文件,写入以下代码
        ```
        import { prop, ModelOptions } from '@typegoose/typegoose'
        import { ApiProperty } from '@nestjs/swagger'

        // 给数据修改增加时间戳
        @ModelOptions({
          schemaOptions:{
            timestamps: true
          }
        })

        export class User{
            @ApiProperty({ description: '用户名', example: 'user'})
            @prop()
            username:string

            @ApiProperty({description: '密码', example: 'pwd'})
            @prop()
            password:string
        }
        ```  
        在 db/src/db.module.ts 文件中引入并导出 user模块 
        ```
        import { User } from './models/user.model';

        const models = TypegooseModule.forFeature([User])

        @Module({
          imports:[
            ...,
            models
          ],
          providers: [DbService],
          exports: [DbService, models],
        })
        ```
    4. 使用用户模块，并使用crud自动生成增删改查
      在src/users/user.controller.ts 中引入 数据库User / crud
      ```
        import { User } from '@libs/db/models/user.model';
        import { ApiTags } from '@nestjs/swagger';
        import { Crud } from 'nestjs-mongoose-crud'
        @Crud({
          model: User
        })
        @ApiTags('用户')
        @Controller('users')
        export class UsersController {
          constructor(@InjectModel(User) private readonly model){ }
        }
      ```
      保存，再次打开http://localhost:3000/api-docs/ ,如下图  

      <img src="https://person-study.oss-cn-beijing.aliyuncs.com/swagger-crud.png">
