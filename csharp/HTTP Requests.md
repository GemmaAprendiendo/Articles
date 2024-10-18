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