---
title: springboot集成es
toc: true
date: 2021-08-22 19:11:09
tags: es
url: es-springboot
---

Springboot集成es是重点，今天跟着狂神一起学习。

<!--more-->

# 官方文档

https://www.elastic.co/guide/en/elasticsearch/client/index.html

第一步：找到原生的API

```xml
  <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
```

测试API

```java
package com.mrhy.es;

import com.alibaba.fastjson.JSON;
import com.mrhy.es.pojo.User;
import org.elasticsearch.action.admin.indices.delete.DeleteIndexRequest;
import org.elasticsearch.action.bulk.BulkRequest;
import org.elasticsearch.action.bulk.BulkResponse;
import org.elasticsearch.action.delete.DeleteRequest;
import org.elasticsearch.action.delete.DeleteResponse;
import org.elasticsearch.action.get.GetRequest;
import org.elasticsearch.action.get.GetResponse;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.index.IndexResponse;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.action.support.master.AcknowledgedResponse;
import org.elasticsearch.action.update.UpdateRequest;
import org.elasticsearch.action.update.UpdateResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.client.indices.CreateIndexRequest;
import org.elasticsearch.client.indices.CreateIndexResponse;
import org.elasticsearch.client.indices.GetIndexRequest;
import org.elasticsearch.common.unit.TimeValue;
import org.elasticsearch.common.xcontent.XContentType;
import org.elasticsearch.index.query.QueryBuilder;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.fetch.subphase.FetchSourceContext;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.io.IOException;
import java.util.ArrayList;
import java.util.Locale;
import java.util.UUID;
import java.util.concurrent.TimeUnit;

/**
 * @description:
 * @author: cooper
 * @date: 2021/8/22 11:25 下午
 */
@SpringBootTest
public class EsTest {
    @Autowired
    private RestHighLevelClient restHighLevelClient;


    public static final String INDEX = "mrhy_index";

    /**
     * @description 测试创建索引
     * @author cooper
     * @date 2021/8/23 9:41 下午
     */
    @Test
    public void createIndex() throws IOException {
        // 创建索引请求
        CreateIndexRequest indexRequest = new CreateIndexRequest(INDEX);
        // 执行请求
        CreateIndexResponse createIndexResponse = restHighLevelClient.indices().create(indexRequest, RequestOptions.DEFAULT);

        // 获取结果
        System.out.println(createIndexResponse);
    }

    /**
     * @description 测试删除索引
     * @author cooper
     * @date 2021/8/23 9:42 下午
     */
    @Test
    public void deleteIndex() throws IOException {
        DeleteIndexRequest deleteIndexRequest = new DeleteIndexRequest(INDEX);
        AcknowledgedResponse acknowledgedResponse = restHighLevelClient.indices().delete(deleteIndexRequest, RequestOptions.DEFAULT);
        System.out.println(JSON.toJSONString(acknowledgedResponse));

    }

    /**
     * @description 测试获取索引
     * @author cooper
     * @date 2021/8/23 9:50 下午
     */
    @Test
    public void existIndex() throws IOException {
        GetIndexRequest indicesExistsRequest = new GetIndexRequest(INDEX);
        boolean acknowledgedResponse = restHighLevelClient.indices().exists(indicesExistsRequest, RequestOptions.DEFAULT);
        System.out.println(JSON.toJSONString(acknowledgedResponse));

    }

    /**
     * @description 测试创建文档
     * @author cooper
     * @date 2021/8/29 5:11 下午
     */

    @Test
    public void testCreateDocument() throws IOException {
        User cooper = new User("cooper", 25);
        IndexRequest indexRequest = new IndexRequest(INDEX);
        indexRequest.id(UUID.randomUUID().toString().toLowerCase(Locale.ROOT));
        indexRequest.timeout("1s");
        indexRequest.source(JSON.toJSONString(cooper), XContentType.JSON);
        IndexResponse index = restHighLevelClient.index(indexRequest, RequestOptions.DEFAULT);
        System.out.println(JSON.toJSONString(index));
        System.out.println(index.status());
        System.out.println(index.getId());
    }

    /**
     * @description 测试判断和获取document
     * @author cooper
     * @date 2021/8/29 5:36 下午
     */
    @Test
    public void testExistAndGetDocument() throws IOException {
        GetRequest request = new GetRequest(INDEX, "1");
        // FetchSourceContext 判断是否需要创建资源。判断是否存在的时候一定设置为false，保证效率。
        request.fetchSourceContext(new FetchSourceContext(true));
        boolean exists = restHighLevelClient.exists(request, RequestOptions.DEFAULT);
        if (exists) {
            GetResponse response = restHighLevelClient.get(request, RequestOptions.DEFAULT);
            System.out.println(response.getSourceAsString());
        }
    }

    /**
     * @description 更新es中的文档
     * @author cooper
     * @date 2021/8/29 5:59 下午
     */
    @Test
    public void updateDocument() throws IOException {
        UpdateRequest updateRequest = new UpdateRequest(INDEX, "1");
        updateRequest.doc(JSON.toJSONString(new User("cooper", 18)), XContentType.JSON);
        UpdateResponse update = restHighLevelClient.update(updateRequest, RequestOptions.DEFAULT);
        System.out.println(JSON.toJSONString(update));
    }

    /**
     * @description 删除文档
     * @author cooper
     * @date 2021/8/29 6:04 下午
     */
    @Test
    public void testDeleteDocument() throws IOException {
        DeleteRequest deleteRequest = new DeleteRequest(INDEX, "1");
        DeleteResponse delete = restHighLevelClient.delete(deleteRequest, RequestOptions.DEFAULT);
        System.out.println(delete.status());
    }
    
    /**
     * @description 批量插入数据
     * @author cooper
     * @date 2021/8/29 6:06 下午
     */
    @Test
    public void testBulkRequest() throws IOException{
        BulkRequest bulkRequest = new BulkRequest(INDEX);
        bulkRequest.timeout("10s");
        ArrayList<User> users = new ArrayList<>();
        users.add(new User("张三",12));
        users.add(new User("李四",13));
        users.add(new User("王五",14));
        users.add(new User("赵六",15));
        // 批处理请求
        for (User user : users) {
            bulkRequest.add(new IndexRequest().id(UUID.randomUUID().toString().toLowerCase(Locale.ROOT)).source(JSON.toJSONString(user),XContentType.JSON));
        }
        BulkResponse bulk = restHighLevelClient.bulk(bulkRequest, RequestOptions.DEFAULT);
        System.out.println(bulk.status());
    }

    /**
     * @description 搜索
     * SearchRequest: 搜索请求
     * SearchSourceBuilder：条件构造
     * QueryBuilder：查询
     * HighlightBuilder：高亮
     * @author cooper
     * @date 2021/8/29 6:17 下午
     */
    @Test
    public void testSearch() throws IOException {
        SearchRequest searchRequest = new SearchRequest(INDEX);
        // 构建搜索条件
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
        // 查询条件，我们可以用QueryBuilder来实现
        QueryBuilder queryBuilder= QueryBuilders.termQuery("name","cooper");
        sourceBuilder.query(queryBuilder);
        sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));
        searchRequest.source(sourceBuilder);
        SearchResponse response = restHighLevelClient.search(searchRequest, RequestOptions.DEFAULT);
        for (SearchHit hit : response.getHits().getHits()) {
            System.out.println(hit.getSourceAsMap());
        }
    }



}

```





