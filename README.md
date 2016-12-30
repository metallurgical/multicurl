# Multi Curl
Multi Curl with batch and parallel request. As the name suggest, you can make a batch request or parallel request at once. 

# Batch Request
Will split array data provided, and request part by part to minimize workload during request instead of requesting at once. Defining how much the number of data per request is at your own choices. The batch request simply divided array of url(s) part by part and doing the request one by one until finish. While waiting for next queue of request, you can provide callback for each batch to execute.

# Paraller Request
Passed in the whole set of array data and will request all at once.

# Installation

Library can be installed using composer, just require it :

    composer require arch/multicurl
    
# Usage Example

1) Inside file just include `Arch\MultiCurl\MultiCurl` at the top of page: 

```Php
<?php

use Arch\MultiCurl\MultiCurl;

?>
```

2) Instantiate library and call the available method :

```Php
<?php

use Arch\MultiCurl\MultiCurl;

$baseUrl = 'https://maps.googleapis.com/maps/api/geocode/json?'; 
$key = '&key=Google_Map_Keys'; 

$urls = array( 
    $baseUrl . 'latlng=3.114536,101.659844'.$key, 
    $baseUrl . 'latlng=3.084217,101.595965'.$key, 
    $baseUrl . 'latlng=3.064248,101.437435'.$key,
    $baseUrl . 'latlng=3.114536,101.659844'.$key
); 

$perRequest = 3; // 3 endpoints per request

$curlMulti = new MultiCurl( $urls, $perRequest ); 
$curlMulti->execute( function ( $data ) {  // execute curl with callback for each request
    echo count($data); 
});
```

# Options

1) `new MultiCurl( $urls, $perRequest )` accept two parameter for initial configuration. 

- **$urls** = ( Required ). Array of url(s)/endpoint(s)
- **$perRequest** = ( Optional ). Indicate the number of data(length) per request and default is 5. This config is for batch request only as the default option for curl operation is a batch instead of requesting the whole set of url(s). You can change this option, see below section.

2) Change default operation for curl either to batch request or all at once by calling `setBatchOperation()` method, default is `true` for batch request operation :

```Php
$curlMulti = new MultiCurl( $urls, $perRequest );
$curlMulti->setBatchOperation( false );
$result = $curlMulti->execute();
print_r( $result );
```

3) Providing callback in `execute( fn(x) )` for batch request whenever its finish executing the request for each batch. As example, we have 3 batch requests, and the callback provided will run 3 times for each batch with the result it received. This callback have 1 arguments for result set
receive from the endpoints. This callback is optional.

- **$data** = Results received from the endpoints

```Php
// As example, we have 3 batch requests
$curlMulti = new MultiCurl( $urls, $perRequest ); 
$curlMulti->execute( function ( $data ) {  // execute curl with callback for each request, will run 3 times
    echo count($data); 
});
```



