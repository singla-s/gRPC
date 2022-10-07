# gRPC
implement gRPC client and server and establish communication between them.

In this tutorial, you:
```
Create a gRPC Server.
Create a gRPC client.
Test the gRPC client with the gRPC Greeter service.
```

# Create a gRPC service

1. Start Visual Studio 2022 and select Create a new project.
2. In the Create a new project dialog, search for gRPC. Select ASP.NET Core gRPC Service and select Next.
3. In the Configure your new project dialog, enter GrpcGreeter for Project name. It's important to name the project GrpcGreeter so the namespaces match when you copy and paste code.
4. Select Next.
5. In the Additional information dialog, select .NET 6.0 (Long-term support) and then select Create.

## Files explanation:
1. Protos/greet.proto: defines the Greeter gRPC and is used to generate the gRPC server assets. For more information, see Introduction to gRPC.
2. Services folder: Contains the implementation of the Greeter service.
3. appSettings.json: Contains configuration data such as the protocol used by Kestrel. For more information, see Configuration in ASP.NET Core.
4. Program.cs, which contains:
The entry point for the gRPC service. For more information, see .NET Generic Host in ASP.NET Core.
Code that configures app behavior. For more information, see App startup.

# Create the gRPC client in a .NET console app

1. Open a second instance of Visual Studio and select Create a new project.
2. In the Create a new project dialog, select Console Application, and select Next.
3. In the Project name text box, enter GrpcGreeterClient and select Next.
4. In the Additional information dialog, select .NET 6.0 (Long-term support) and then select Create.

## Install Packages

Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools

## Add Proto file from service

1. Add greet.proto
2. Create a Protos folder in the gRPC client project.
3. Copy the Protos\greet.proto file from the gRPC Greeter service to the Protos folder in the gRPC client project.
4. Update the namespace inside the greet.proto file to the project's namespace.

## Edit the GrpcGreeterClient.csproj project file:

1. Right-click the project and select Edit Project File.
2. Add an item group with a <Protobuf> element that refers to the greet.proto file:
  ```
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

## Create the Greeter client

1. Build the client project to create the types in the GrpcGreeterClient namespace.
2. Update the gRPC client Program.cs file with the following code.
  ```
  using System.Threading.Tasks;
  using Grpc.Net.Client;
  using GrpcGreeterClient;

  // The port number must match the port of the gRPC server.
  using var channel = GrpcChannel.ForAddress("https://localhost:7042");
  var client = new Greeter.GreeterClient(channel);
  var reply = await client.SayHelloAsync(
                    new HelloRequest { Name = "GreeterClient" });
  Console.WriteLine("Greeting: " + reply.Message);
  Console.WriteLine("Press any key to exit...");
  Console.ReadKey();
  ```

3. In the preceding highlighted code, replace the localhost port number 7042 with the HTTPS port number specified in Properties/launchSettings.json within the GrpcGreeter service project.

# Test the execution

1. In the Greeter service, press Ctrl+F5 to start the server without the debugger.
2. In the GrpcGreeterClient project, press Ctrl+F5 to start the client without the debugger.

# Output

```
Greeting: Hello GreeterClient
Press any key to exit...
```
