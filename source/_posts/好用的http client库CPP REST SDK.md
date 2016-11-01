---
title: 好用的http client库CPP REST SDK
date: 2016-11-01 22:05:41
categories: C++11
tags: 
    - cpprestsdk
    - http client
    - PPL
---

![此处输入图片的描述][1]

前言
-----

C++中http client库本身就少，好用的就更少了，在了解微软开源的[CPP REST SDK][2]库之前，我知道的C++ http client库有[libcurl][3]（这个是C语言的），Qt的[QNetworkAccessManager][4]，还有VC++ http client，Qt的QNetworkAccessManager库我在开发[CZPlayer][5]的时候用来下载过音乐、专辑图片和歌词，不得不说Qt提供的API还是比较好用的，如果不涉及界面开发，难道我们在linux上就只能用libcurl，在windows上就用VC++的http client？答案是否定的，在绝望之际CPP REST SDK出现在我的眼前，CPP REST SDK是微软开源的基于PPL的异步http client，网络层使用的是Boost.Asio，跨平台，并且支持json解析，在使用CPP REST SDK之前要确保你已经安装了boost和openssl，下面是微软官方提供的例子。

<!--more-->

微软官方例子
-----

```cpp
#include <cpprest/http_client.h>
#include <cpprest/filestream.h>

using namespace utility;                    // Common utilities like string conversions
using namespace web;                        // Common features like URIs.
using namespace web::http;                  // Common HTTP functionality
using namespace web::http::client;          // HTTP client features
using namespace concurrency::streams;       // Asynchronous streams

int main(int argc, char* argv[])
{
    auto fileStream = std::make_shared<ostream>();

    // Open stream to output file.
    pplx::task<void> requestTask = fstream::open_ostream(U("results.html")).then([=](ostream outFile)
    {
        *fileStream = outFile;

        // Create http_client to send the request.
        http_client client(U("http://www.bing.com/"));

        // Build request URI and start the request.
        uri_builder builder(U("/search"));
        builder.append_query(U("q"), U("cpprestsdk github"));
        return client.request(methods::GET, builder.to_string());
    })

    // Handle response headers arriving.
    .then([=](http_response response)
    {
        printf("Received response status code:%u\n", response.status_code());

        // Write response body into the file.
        return response.body().read_to_end(fileStream->streambuf());
    })

    // Close the file stream.
    .then([=](size_t)
    {
        return fileStream->close();
    });

    // Wait for all the outstanding I/O to complete and handle any exceptions
    try
    {
        requestTask.wait();
    }
    catch (const std::exception &e)
    {
        printf("Error exception:%s\n", e.what());
    }

    return 0;
}
```

上面的例子主要内容是访问一个网站并将该内容保存在results.html里面，这里用到了微软自家的PPL并行计算库（还有一个是英特尔的TBB），该例子使用lambda表达式作为异步回调的handler，打开文件流、请求、回应和关闭文件流都是异步的，为了方便起见，我们将该例子改为同步方式。

使用同步方式
-----

```cpp
auto fileStream = std::make_shared<ostream>();
ostream outFile = fstream::open_ostream(U("results.html")).get();
*fileStream = outFile;

// Create http_client to send the request.
http_client client(U("http://www.bing.com/"));

// Build request URI and start the request.
uri_builder builder(U("/search"));
builder.append_query(U("q"), U("cpprestsdk github"));
http_response response = client.request(methods::GET, builder.to_string()).get();
// Write response body into the file.
response.body().read_to_end(fileStream->streambuf()).get();
fileStream->close().get();
```

使用同步方式的代码比较清晰，可以看到每一个函数调用后面都会调用get函数，因为CPP REST SDK是基于PPL的，所以在request、read_to_end、close等函数调用后都会返回一个Task对象，而Task里面的get和wait函数是等待任务执行完成。

实战
-----

我之前在看别人博客的时候看到每一篇博客前都有一副图，看起来比较好看，听博主讲他是使用bing的每日一图，后来我也效仿，我在主题之家找到了自己比较喜欢的图片，最开始自己写博客的时候都是直接去主题之家一页一页的找看，然后再下载图片，写了几篇博客之后感觉每次都要去下载图片感觉有点low，所以我决定直接将主题之家的某类型的图片都下载下来，这样选图片就方便多了，下面是我在主题之家抓图的代码。

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <fstream>
#include <memory>
#include <regex>
#include <cpprest/http_client.h>
#include <cpprest/filestream.h>
#include <cpprest/containerstream.h>

// 请求并解析url
bool get_result(const std::string& url, const std::string& pattern, std::vector<std::string>& vec)
{
	try
	{
		web::http::client::http_client client(web::uri(utility::conversions::to_string_t(url)));
		web::http::http_response response = client.request(web::http::methods::GET).get();

		concurrency::streams::stringstreambuf buffer;
		response.body().read_to_end(buffer).get();
		std::string& str = buffer.collection();
        
        // 使用C++11提供的正则表达式库
		std::regex r(pattern);
		for (std::sregex_iterator iter(str.begin(), str.end(), r), end; iter != end; ++iter)
		{
			std::cout << iter->str() << std::endl;
			vec.emplace_back(iter->str());
		}
	}
	catch (std::exception& e)
	{
		std::cout << "Exception: " << e.what() << std::endl;
		return false;
	}

	return true;
}

// 获取图片
bool get_result(const std::string& url, std::string& picture)
{
	try
	{
		web::http::client::http_client client(web::uri(utility::conversions::to_string_t(url)));
		web::http::http_response response = client.request(web::http::methods::GET).get();

		concurrency::streams::stringstreambuf buffer;
		response.body().read_to_end(buffer).get();
		picture = buffer.collection();
	}
	catch (std::exception& e)
	{
		std::cout << "Exception: " << e.what() << std::endl;
		return false;
	}

	return true;
}

// 保存图片
bool write_to_file(const std::string& file_path, const std::string& data)
{
	try
	{
		std::ofstream file;
		file.open(file_path, std::ios::out | std::ios::binary);
		if (!file.good())
		{
			return false;
		}
		file.write(data.c_str(), data.size());
		file.close();
	}
	catch (std::exception& e)
	{
		std::cout << "Exception: " << e.what() << std::endl;
		return false;
	}

	return true;
}

int main()
{
    // [1] 请求每一页，将子页面的url保存在sub_url_vec里面
	std::vector<std::string> sub_url_vec;
	std::string pattern = "/desk/[0-9]+.htm";
	for (int i = 1; i <= 32; ++i)
	{
		// 创意主题
		std::string url = "http://www.51ztzj.com/dbizhi/category_27_" + std::to_string(i) + ".htm#content_anchor";
		std::cout << "Start get " << i << " page, url: " << url << std::endl;
		// 请求并解析url
		if (!get_result(url, pattern, sub_url_vec))
		{
			std::cout << "Get " << i << " page failed" << std::endl;
		}
	}

	// 最终的图片url：http://img.51ztzj.com//upload/image/20130220/2013022014_670x419.jpg
	// [2] 将子页面的图片url解析出来放入picture_url_vec
	std::vector<std::string> picture_url_vec;
	pattern = "http://img.51ztzj.com//upload/image/.+/.+_670x419.jpg";
	for (std::size_t i = 0; i < sub_url_vec.size(); ++i)
	{
		std::string url = "http://www.51ztzj.com" + sub_url_vec[i];
		std::cout << "Start get " << i + 1 << " sub page, url: " << url << std::endl;
		// 请求并解析url
		if (!get_result(url, pattern, picture_url_vec))
		{
			std::cout << "Get " << i + 1 << " sub page failed" << std::endl;
		}
	}

    // [3] 最后遍历picture_url_vec，然后一个一个的下载图片
	for (std::size_t i = 0; i < picture_url_vec.size(); ++i)
	{
		std::cout << "Start download " << i + 1 << " picture, url: " << picture_url_vec[i] << std::endl;
		std::string picture;
		// 获取图片
		if (!get_result(picture_url_vec[i], picture))
		{
			std::cout << "Download " << i + 1 << " picture failed" << std::endl;
		}

		std::string file_path = "./download/" + std::to_string(i) + ".jpg";
		// 保存图片
		if (!write_to_file(file_path, picture))
		{
			std::cout << "Write to file failed: " << i + 1 << std::endl;
		}
	}

    return 0;
}
```


  [1]: https://raw.githubusercontent.com/chxuan/images/master/blog/2016/11/cpprestsdk.jpg
  [2]: https://github.com/Microsoft/cpprestsdk
  [3]: https://curl.haxx.se/
  [4]: http://doc.qt.io/qt-5/qnetworkaccessmanager.html
  [5]: https://github.com/chxuan/CZPlayer
