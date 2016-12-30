# Multi Curl
Multi Curl with batch and parallel request. As the name suggest, you can make a batch request or parallel request at once. 

# Batch Request
Will split array data provided, and request part by part to minimize workload during request instead of requesting at once. Defining how much
the number of data per request is at your own choices.

# Paraller Request
Passed in the whole set of array data and will request all at once.

# Installation

Library can be installed using composer, just require it :

    composer require arch/multicurl
