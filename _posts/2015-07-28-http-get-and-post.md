---
layout: single
title: 对使用http请求的GET和POST的一点思考 
date: 2015-7-28 20:30
comments: true
categories: Development
---

对使用http请求的GET和POST的一点思考

在web开发中，一个http请求方法有GET / HEAD / PUT / DELETE / OPTIONS / CONNECT几个形式，对此不多究。我今天主要想谈谈常见的GET和POST两种方法的使用思考。

先来讲一个我最近做的一个 Ruby on Rails 项目，在那个项目中，用户(User)可以选择某一个数据集合(DataSet)项目参与其中做任务(Task)。模型关系如下

    class User < ActiveRecord::Base
      has_many :tasks       
    end

    class DataSet < ActiveRecord::Base

      def user_task(user)
        user.tasks.where(data_set_id: self.id).first
      end

    end

    class Task < ActiveRecord::Base
      belongs_to :user
      belongs_to :data_set
    end

在数据集合(DataSet)列表页面，列出多个集合(DataSet)，用户可以选择其中一个参与任务(Task)。如果用户还没有参与其中的某个项目，显示“开始工作”，否则显示“继续工作”，页面部分代码如下：
       
            <% @data_sets.each do |data_set| %>
            <div class="col-sm-6 col-md-4">
              <div class="thumbnail">
                <%= image_tag data_set.logo.url %>
                <div class="caption">
                  <h3><%= data_set.name %></h3>
                  <p><%= data_set.description %></p>
                  <p>
                    <% if data_set.user_task(current_user).present? %>
                      <%= link_to '继续工作', workspace_data_set_path(data_set), class: 'btn btn-primary' %>
                    <% else %>
                      <%= link_to '开始工作', choose_task_data_set_path(data_set), method: :post, class: 'btn btn-primary' %>
                    <% end %>
                  </p>
                </div>
              </div>
            </div>
            <% end %>
      
DataSetsController中的部分代码如下：

    class DataSetsController < ApplicationController
      before_action :authenticate_user!
      before_action :set_data_set, only: [:show, :edit, :update, :destroy, :choose_task, :workspace, :mark_picture]

      # GET /data_sets
      # GET /data_sets.json
      def index
        @data_sets = DataSet.all
      end

      # POST /data_sets/1/choose_task
      def choose_task
        if current_user.tasks.where(data_set_id: @data_set.id).first.nil?
          current_user.tasks.build(data_set_id: @data_set.id).save
        end

        redirect_to workspace_data_set_path(@data_set)
      end

      # GET /data_sets/1/workspace
      def workspace   
        # ...
      end
    end 
    
当初在设计这个页面上逻辑的时候，一开始以为直接用一个方法请求就搞定了,页面点击“开始工作”或者“继续工作”按钮，直接GET请求跳转到用户工作台链接/data_sets/{id}/workspace，在workspace action中加入额外的逻辑判断是否要创建用户的任务(Task)。我接着认真思考了一下，发现这样不妥。原因有是GET /data_sets/{id}/workspace中的逻辑不纯粹， 与它的URL本身语义不符合。也不利于测试。

那也许有人会说我将workspace这个action改成POST显示可以吗？答案也是不可以的，因为你点击按钮进入这个页面后，你如果刷新当前的workspace页面，浏览器会提示是否重复提交请求的提示，给用户的体验也不好。实质是这个请求不可cache。另外在其它页面地方也不能通过一般的a link的方式进入workspace页面，不可传播。

现在我将这个逻辑分开写了 POST /data_sets/{id}/choose_task 和 GET /data_sets/{id}/workspace 两个请求，代码逻辑是很合理的。用户可以随意刷新他的工作台，而不用担心刷新会改变什么数据。

啰嗦了这么多，其实这是一个很小的问题，但是容易忽略。之前在一个公司维护一个项目，发现页面中做的一个商品多条件查询表单居然是用POST请求的。每次在浏览器中查询商品后，按F5刷新，总是提示是否重复提交请求，用户体验很不好。另外我还没有办法把我查询找到的商品结果页面发给我其他的朋友。

现在我来总结一下http请求的GET和POST方法使用。

首先如名词所示，GET是用来从服务器上请求指定的资源，请求携带的信息都是体现在URL参数上面，如/products?category=1&color=red。URL长度有限制。

POST是向服务器中提交数据，请求携带的信息一般放在http请求的消息体中，上传的数据长度在服务器端可以限制。

从这里应该可以明确一点：GET请求应该不会带来影响的，重复请求不会改动服务器上的资源，它只负责取数据。POST请求是想服务器提交数据，重复提交动作对服务器资源是有影响的。那么GET请求是可以被前端缓存的，而POST请求不行。

在用户的浏览器中，GET请求的URL是可以被保存书签的，POST请求不行。GET请求的URL是有历史记录的，可以前进 / 后退 / 刷新。POST请求则不行。

完。
