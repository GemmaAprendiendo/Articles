# HTTP requests and responses basics

## Some Background Info
Types of requests.

Create — POST
Read — GET
Update — PUT
Delete — DELETE

There is also a PATCH option but it’s not that common.

The above (right side) are the type of requests we can get from a browser or some application accessing our APIs. We use codes to communicate how the request is going. Some of the most common are:

200 OK
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
406 Not Acceptable
500 Server Error
503 Service Unavailable
201 Created
202 Accepted
204 No Content
405 Method Not Allowed
415 Unsupported Media Type

The System.Net.Http.HttpClient class sends HTTP requests and receives HTTP responses from a 
resource identified by a URI. An HttpClient instance is a collection of settings that's applied to all requests executed by that instance.

It is a good idea to use it in a USING statement so it's not left around when the request is done.  You can use it per call (better in a using) but,
HttpClient is intended to be instantiated once per application.

```
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
		//Have a using for it
        using (var client = new HttpClient())
        {
			//make the async call with the client and await the response.  Then check the return.
            HttpResponseMessage response = await client.GetAsync("http://example.com/"); //Could send CancellationTokens to this GetAsync methods
            if (response.IsSuccessStatusCode)
            {
				//if the response was OK, we can await for whatever the response may contain from the call.
				//use the appropriate type to match what is being returned (could be response.Content.ReasAsAsync<SOMETYPE>();
                string responseBody = await response.Content.ReadAsStringAsync();
                Console.WriteLine(responseBody);
            }
            else
            {
                Console.WriteLine("HTTP Error: " + response.StatusCode);
            }
        }
    }
}
```
You can set attributes that will be added to the request.

```
...
class Program
{
    static async Task Main(string[] args)
    {
        string url = "https://api.example.com/something";
        string token = "your_access_token_here"; .

        using (var httpClient = new HttpClient())
        {
            // Setting the base address can simplify subsequent calls if they are to the same host
            httpClient.BaseAddress = new Uri(url);

            // Adding the Authorization header with a Bearer token
            httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("usernameorsomething", token);

            try
            {
                // Send the GET request
                HttpResponseMessage response = await httpClient.GetAsync("");

                // Check if the request was successful
                if (response.IsSuccessStatusCode)
                {
                    // Read the content of the response
                    string responseBody = await response.Content.ReadAsStringAsync();
                    ...
                }                
            }
            catch (HttpRequestException e)
            {
                Console.WriteLine($"An error occurred: {e.Message}");
            }
        }
    }
}
```

You can clear and set request headers with 
```
client.DefaultRequestHeaders.Accept.Clear();
client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/vnd.github.v3+json"));
```

If you are getting json back:

```
//sends the http get request
var json = await client.GetStringAsync("https://whatever");
```
Could have also done

```
await using Stream stream = await client.GetStreamAsync("https://whatever");
var YourClassVar = await JsonSerializer.DeserializeAsync<List<YourClass>>(stream);
	
```

When creating a new HttpClient you can give it a base class.  When you use this HttpClient, you will not have to give the base address again.

```
private static HttpClient sharedClient = new()
{
    BaseAddress = new Uri("https://jsonplaceholder.typicode.com"),
};
```

The User-Agent request header is a string that lets servers and network peers identify the 
application, operating system, vendor, and/or version of the requesting user agent.
For example, when requesting from the GitHub API, Most GitHub REST API endpoints specify 
that you should pass an Accept header with a value of application/vnd.github+json. The value of the Accept header is a media type.
```
client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/vnd.github.v3+json"));
```

All API requests for GitHub must include a valid User-Agent header. The User-Agent header identifies the user or application that is making the request.
GitHub recommends using your GitHub username, or the name of your application, for the User-Agent header value.

```
client.DefaultRequestHeaders.Add("User-Agent", "YOUR APP HERE");
```
We can get a HttpResponseMessage but we can also send a HttpRequestMessage.
```
var msg = new HttpRequestMessage(HttpMethod.Get, url);
var res = await client.SendAsync(msg);

var content = await res.Content.ReadAsStringAsync();
```
If the api needs parameters, you can add them this way.
```
 ".....?name=JohnDoe&occupation=gardener";
 ```
 You can set the timeout for the request with
 ```
 \\3 minutes
 httpClient.Timeout = TimeSpan.FromMinutes(3);
 ```
 POST requests are often sent via a post form. The type of the body of the request is indicated by the Content-Type header.
 ```
 data //data of whatever type you have
 var res = await client.PostAsync(url, new FormUrlEncodedContent(data));
 ```
 Don't forget that many times you will need Authorization
 
```
var authToken = Encoding.ASCII.GetBytes($"{userName}:{passwd}");
client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", Convert.ToBase64String(authToken));
```		

