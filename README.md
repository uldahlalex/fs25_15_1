# Software Testing

Recap + new revisions to API testing.

## Agenda
- Live demo: Writing tests using the new "formula"
- Presentation about revised testing setup

## Topics
- ConfigureServices, DI in testing, WebAppFactory
- Mermaid charts for most basic fullstack operations
- Test WS client (+ Short mention of Test MQTT client for IoT projects)
- Short revisit to automating the tests


<br />

# Today's Activities: Using the testing Formula

**Task: Write tests using the revised setup**

Example: Make a new Controller method + Service method to do things like CRUD operations and broadcasts. Then make tests that "Trigger" these operations and assert on things like received WS messages, DB state, etc.

```csharp
[TestFixture]
public class RestTriggeredTests
{
    private HttpClient _httpClient;
    private IServiceProvider _scopedServiceProvider;

    [SetUp]
    public void Setup()
    {
        var factory = new WebApplicationFactory<Program>()
            .WithWebHostBuilder(builder =>
            {
                builder.ConfigureServices(services =>
                {
                    //Here you can make your own method to modify services or use mine from fs25_14_2
                    services.DefaultTestConfig();
                });
            });

        _httpClient = factory.CreateClient();
        _scopedServiceProvider = factory.Services.CreateScope().ServiceProvider;
    }

    [TearDown]
    public void TearDown()
    {
        _httpClient?.Dispose();
    }

    [Test]
    public async Task GivenThis_WhenThatHappens_ThenThisIsTheResult()
    {
        //this is where you write the test   
    }

    //optionally more tests down here
}
```