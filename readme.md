## Kyruus PHP SDK

This is a PHP HTTPS-based API wrapper for the ProviderMatch search API provided by Kyruus.

This SDK provides helper methods to enable searching for doctors and other healthcare providers, as configured for 
your organization. To use this, you must first have a Kyruus account.

Once you have an account and have enabled API access, you can find out more about the search APIs 
at http://support.kyruus.com.

### To Install

`composer require asymmetrik/kyruus-php-sdk`

### How to use

Before creating a client you must create a `RequestCoordinator` which simply is the OAuth wrapper for the SDK Client.
 
```
$coordinator = new Asymmetrik/Kyruus/Http/RequestCoordinator('https://kyruus-root-url', 'oauthuser', 'oauthpass')
```

You can then pass your coordinator to the SDK client with your organization

```
$client = new Asymmetrik/Kyruus/SDK/Client($coordinator, 'myorg');
```

### Building queries

The SDK currently employs no actual query builder and simply appends the data to the overall search query.

If your search only deals with providers you can directly call it from the SDK Client, alternatively you could get 
an instance of the builder and then call providers yourself

```
$client->providers(); //QueryBuilder instance

$client->builder()->providers();
```

What the builder offers is a chainable interface to the API endpoint.

```
$query = $client->providers()
                ->per_page(20)
                ->page(2)
                ->facet('specialties')
                ->name('lemma');
```

From your query you can either directly get the results with `get` or `compile` your query into a string and pass it 
somewhere else. 

```
$query->compile(); //https://root-url/endpoint/org/providers?attributes

$query->get(); //If successful you will get a json decoded response
```
