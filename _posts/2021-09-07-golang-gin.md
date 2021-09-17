---
title: Gin复用相同的路由
tags: TeXt
article_header:
  type: cover
---

### Gin 中多个相同的项目复用相同的路由
- 例如 多个类别都要创建订单 但是具体细节都不一样 只是路由需要统一
   - 入口注册路由
  ```golang
     webServer := web.NewWeb()
     restful.Init(webServer, m, cfg)
  ```
  - 注册全部模块
  ```golang
    func Init(w *web.Web, m *module.Module, cfg *config.Config) {
          middlewares.Init(cfg)
	        w.Get("/healthy", checkHealthy)
	        w.HandleFunc("OPTIONS", "/*", doNothing)
	        mysql.Init(w, m, cfg)
	        clickhouse.Init(w, m, cfg)
	        redis.Init(w, m, cfg)
	        team.Init(w, m, cfg)
	        mongodb.Init(w, m, cfg)
	        elasticsearch.Init(w,m ,cfg)
    }
  ```

  - 具体各个注册路由期依赖
  ```golang
    func registerAuth(r web.Router, m *module.Module, cfg *config.Config) {
	        authHandler := auth.NewAuthHandler(m.Elasticsearch.AuthManager, m.AccountClient, m.KAEClient, m.Elasticsearch.UnitQueryer)
	        auth.RegisterURLs(r, m.Elasticsearch.TypeName, authHandler)
    }

    func registerOpLog(r web.Router, m *module.Module, cfg *config.Config) {
          opLogHandler := oplog.NewOpLogHandler(m.Elasticsearch.OpLogManager, m.Elasticsearch.AuthManager, m.Elasticsearch.UnitQueryer)
          oplog.RegisterURLs(r, m.Elasticsearch.TypeName, opLogHandler)
    }

    func registerUnit(r web.Router, m *module.Module, cfg *config.Config) {
	        unitHandler := newUnitHandler(m.Elasticsearch.UnitManager, m.KAEClient, m.Elasticsearch.AuthManager)
	        dbunit.RegisterURLs(r, m.Elasticsearch.TypeName, unitHandler)
    }

    func registerOrder(r web.Router, m *module.Module, cfg *config.Config) {
	        orderHandler := newOrderHandler(m.Elasticsearch.OrderManager, m.KAEClient, m.AccountClient, m.Elasticsearch.AuthManager, m.Elasticsearch.UnitQueryer)
	        order.RegisterURLs(r, m.Elasticsearch.TypeName, orderHandler)
	  }
    func Init(w *web.Web, m *module.Module, cfg *config.Config) {
	     registerOpLog(w, m, cfg)
	     registerAuth(w, m, cfg)
	     registerUnit(w, m, cfg)
	     registerOrder(w, m, cfg)
   }
  ```
  - 复用注册注册接口
  ```golang
     func RegisterURLs(r web.Router, typ string, orderHandler *OrderHandler) {
	   	 	r.Clear()
	   	 	r.Append(middlewares.GetSessionMiddleware())
	   	 	r.Append(middlewares.NewParseParamsMiddleware())

	   	 	p := fmt.Sprintf
		   	 /////////////////////////////////////////unit////////////////////////////////////////
	   	 	r.HandleFunc("POST", p("/api/dbops/%s/order", typ), orderHandler.CreateOrder)

	   	 r.Clear()
	   	 r.Append(middlewares.NewParseParamsMiddleware())
	     r.HandleFunc("GET", p("/api/dbops/%s/order/{order_id:\\d+}/running_logs/ws", typ), orderHandler.ReadOrderRunningLogs)
  }
  ```
<!--more-->
