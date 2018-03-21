---
layout: post
title: 'Java 實作 Http-Request'
tags: ["java"]
date: 2018-03-21 17:25
comments: true
categories: 
- "學習筆記"
---
# Java 實作 Http-Request
之前專案有實作Java-Http-Request，以下紀錄example
## curl example
```
curl -s -X POST \
  http://localhost:4000 \
  -H "authorization: Bearer token" \
  -H "content-type: application/json" \
  -d '{
	"fcn":"fcn",
	"args":["a","b"]
}'
```
## Java example
```
import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.nio.charset.StandardCharsets;

class TempController {
	def send(){
			String body = '{"fcn":"fcn","args":["a","b"]}'
			String auth = "Bearer token"
			String urlParameters  = body;
			byte[] postData = urlParameters.getBytes( StandardCharsets.UTF_8 );
			int postDataLength = postData.length;
			String checkurl = "http://localhost:4000";
			try
			{
				URL connectto = new URL(checkurl);
				HttpURLConnection conn = (HttpURLConnection) connectto.openConnection();
				conn.setRequestMethod( "POST" );
				conn.setRequestProperty( "Content-Length", Integer.toString( postDataLength ));
				conn.setRequestProperty( "authorization", auth);
				conn.setRequestProperty( "Content-Type", "application/json");
				conn.setRequestProperty( "charset", "utf-8");
				conn.setUseCaches(false);
				conn.setAllowUserInteraction(false);
				conn.setInstanceFollowRedirects( false );
				conn.setDoOutput( true );
				
				DataOutputStream wr = new DataOutputStream(conn.getOutputStream());
				wr.writeBytes(urlParameters);
				wr.flush();
				wr.close();
				
				int responseCode = conn.getResponseCode();
				
				BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
				StringBuilder sb = new StringBuilder();
				String line;
				
				while ((line = br.readLine()) != null) {
					sb.append(line+"\n");
				}
				br.close();
                //輸出結果
				System.out.println(sb);
			}
			catch (IOException e)
			{
				e.printStackTrace();
			}
	}
}
```