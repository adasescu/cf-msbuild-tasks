
#**Sample Target Configuration**


    <Target Name="BuildAndPush">
    		<PropertyGroup>
    			<User>admin</User>
    			<Password>admin</Password>
    			<Server>http://api.15.126.213.170.xip.io</Server>
    		</PropertyGroup>
    		
    		<LoadYaml ConfigurationFile="C:\test\CloudTestApp-Ng\stackato.yml">
    			<Output TaskParameter="Stack" PropertyName="Stack"/>
    			<Output TaskParameter="AppName" PropertyName="AppName"/>
    			<Output TaskParameter="AppPath" PropertyName="AppPath"/>
    			<Output TaskParameter="Routes" PropertyName="Routes"/>
    			<Output TaskParameter="Memory" PropertyName="Memory"/>
    			<Output TaskParameter="Instances" PropertyName="Instances"/>
    			<Output TaskParameter="Services" PropertyName="Services"/>
    		</LoadYaml>
    
    		<CreateApp Name="$(AppName)" Stack="$(Stack)" Space="TestSpace" User="$(User)" Password="$(Password)" ServerUri="$(Server)">
    			<Output TaskParameter="AppGuid" PropertyName="AppGuid"/>
    		</CreateApp>
    
    		<!--<UpdateApp Memory="512" Instances="1" AppGuid="$(AppGuid)" User="$(User)" Password="$(Password)" ServerUri="$(Server)"></UpdateApp>-->
    
    		<PushApp AppGuid="$(AppGuid)" AppPath="$(AppPath)" Start="true" User="$(User)" Password="$(Password)" ServerUri="$(Server)"></PushApp>
    		
    		<CreateRoutes User="$(User)" Password="$(Password)" ServerUri="$(Server)" Space="TestSpace" Routes="$(Routes)">
    			<Output TaskParameter="RouteGuids" PropertyName="RouteGuids"></Output>
    		</CreateRoutes>
    
    		<BindRoutes User="$(User)" Password="$(Password)" ServerUri="$(Server)" AppGuid="$(AppGuid)" RouteGuids="$(RouteGuids)"></BindRoutes>
    
    		<CreateService User="$(User)" Password="$(Password)" ServerUri="$(Server)" Space="TestSpace" Type="mssql2012" Plan="free" Name="TestService">
    			<Output TaskParameter="ServiceGuid" PropertyName="ServiceGuid"></Output>
    		</CreateService>
    
    		
    		<!-- This task uses the output xml generated by LoadYaml to create more services
    		<CreateServices User="$(User)" Password="$(Password)" ServerUri="$(Server)" Space="TestSpace" Services="$(Services)">
    			<Output TaskParameter="ServicesGuids" PropertyName="ServicesGuids"></Output>
    		</CreateServices>-->
    
    		<BindServices User="$(User)" Password="$(Password)" ServerUri="$(Server)" AppGuid="$(AppGuid)" ServicesGuids="$(ServiceGuid)"></BindServices>
    	</Target>

Sample XML used for create services task 

    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfProvisionedService xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
       <ProvisionedService>
          <Name>service1</Name>
          <Plan>free</Plan>
          <Type>mysql</Type>
       </ProvisionedService>
       <ProvisionedService>
          <Name>service2</Name>
          <Plan>free</Plan>
          <Type>mssql2012</Type>
       </ProvisionedService>
    </ArrayOfProvisionedService>

This xml is ***automatically*** generated by the LoadYaml task if a manifest file is used for deployment