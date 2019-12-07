# Have Your Pi and Eat it Too
#### .NET Core on Raspberry Pi

## Agenda 
1. The Basics
   * Development
   * Deployment
   * Debugging
2. GPIO
   * Control an LED
   * Control a reversible motor
   * Read temperatures
3. Cloud integration
   * Display text on an LCD with Cortana

## The Basics
#### The berry beginning

### Deploy a project to the Pi

1. Create a new web servic
   * `dotnet new razor -o pidemo`
1. Publish as a self-contained package
   * `dotnet publish -r linux-arm`
1. Create target directory on Pi
1. Copy files to Pi
1. Give app permission to run `chmod +x pidemo`
1. `./pidemo`
1. `./pidemo --urls=http://*:5000` // listens on all adapters on port 5000

### Debugging through Visual Studio

1. Attach to process `username@hostname` in connection target, provide login credentials when prompted.
1. That's it. Attach it to the process and go.

## GPIO

#### Interacting with GPIO requires `System.Device.Gpio`, a Microsoft NuGet package.

#### `IoT.Device.Bindings` also a Microsoft package, contains drivers for various devices.

## Cloud Design Patterns
#### What's IoT without the internet?

### Easy Cortana Integration

[If this then that](https://ifttt.com)

Cortana -> IFTTT -> Azure Logic App -> Azure Service Bus -> PI .NET Core App listens to service bus

[GitHub for presentation and demo code](https://github.com/camsoper/have-your-pi)