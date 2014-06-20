PHP Cross Domain (AJAX) Proxy
==============

An application proxy that can be used to transparently transfer all kind of requests ( including of course XMLHTTPRequest ) to any third part domain. It is possible to define a list of acceptable third party domains and you are encouraged to do so. Otherwise the proxy is open to any kind of requests.

Installation
--------------

The proxy is indentionally limited to a single file. All you have to do is to place `proxy.php` under your application

Whenever you want to make a cross domain request, just make a request to http://www.yourdomain.com/proxy.php and specify the cross domain URL by using `csurl` parameter. Obviously, you can add more parameters according to your needs; note that the rest of the parameters will be used in the cross domain request. For instance, if you are using jQuery:

``` JAVASCRIPT
$('#target').load(
	'http://www.yourdomain.com/proxy.php', {
		csurl: 'http://www.cross-domain.com/',
		param1: value1, 
		param2: value2
	}
);
```

It’s worth mentioning that all request methods are working GET, PUT, POST, DELETE are working and headers are taken into consideration. That is to say, headers sent from browser to proxy are used in the cross domain request and vice versa.

You can also specify the URL with the `X-Proxy-URL` header, which might be easier to set with your JavaScript library. For example, if you wanted to automatically use the proxy for external URL targets, for GET and POST requests:

``` JAVASCRIPT
$.ajaxPrefilter(function(options, originalOptions, jqXHR) {
	if(options.proxy){
	    if (options.url.match(/^http?:/)) {
	    	if(!options.headers){
	    		options.headers  = {};
	    	}
	        options.headers['X-Proxy-URL'] = options.url;
	        options.url = '/proxy.php?_' + (new Date()).getTime();
	    }
	}
});
```

Configuration
--------------

For security reasons don't forget to define all the valid requests into top section of `proxy.php` file:
( not mandeta )
``` JAVASCRIPT
$valid_requests = array(
	'http://www.domainA.com/',
	'http://www.domainB.com/path-to-services/service-a'
);
```

Examples:

``` JAVASCRIPT
//POST
$.ajax({
	proxy: true, //flag to use proxy
	type: "POST",
	url: "http://localhost:5555/temp/create",
	data: '{"some":"someone"}',
	contentType: 'application/json',
	success: function(res){
		alert('success' + res);
		console.log(res);
	},
	error: function(req){
		alert('error' + req);
		console.log(req);
	}
});

//GET
$.ajax({
	proxy: true, //flag to use proxy
	type: "GET",
	url: "http://localhost:5555/user/list/539b15a7ae32e25c32939acb",
	headers: {
		"domainId" : "539b15a7ae32e25c32939acb",
		"userId": "539b1834f9e114983644a469"
	},		
	success: function(res){
		alert('success' + res);
		console.log(res);
	},
	error: function(req){
		alert('error' + req);
		console.log(req);
	}
});
 
```
