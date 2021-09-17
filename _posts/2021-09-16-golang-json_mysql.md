---
title: golang 从mysql从查询出来的json无法解析
tags: TeXt
article_header:
type: cover
---

###  在mysql字段content中存入是一段json
- 但是在查询出来后却无法解析 直接报错
  ```go
    func (d *IntlAlertDAO) Query(startTs, endTs time.Time) ([]mo.WaterLogs, error) {

	var records []mo.WaterLogs
	rows,err := d.db.Table(d.tbName).Where("create_time >? and create_time <=? ", startTs, endTs).Rows()
	defer d.db.Close()
	if err != nil {
		return nil,err
	}

	for rows.Next() {
		var id int
		var content,createTime string
		err = rows.Scan(&id, &content,&createTime)
		if err != nil {
			logrus.Error(err)
			continue
		}

		DefaultTimeLoc := time.Local
		loginTime, loginTimeErr := time.ParseInLocation("2006-01-02 15:04:05", createTime, DefaultTimeLoc)
		if loginTimeErr != nil {
			logrus.Error(err)
			continue
		}

		var logContent mo.Content
		str := strings.ReplaceAll(content,"'","\"")
		logContentErr := json.Unmarshal([]byte(str),&logContent)
		if logContentErr != nil {
			logrus.Error(err)
			continue
		}

		records = append(records,mo.WaterLogs{ID: id,Content: logContent,CreateTime: loginTime})

		fmt.Println(loginTime)
	}

	if len(records) == 0 {
		return nil, nil
	}

	return records, nil
  }
  ```

##### 因为golang中不支持单引号字符串的json
 ```go
   strings.ReplaceAll(content,"'","\"")
 ```
### 替换后可以正常被解析

<!--more-->
