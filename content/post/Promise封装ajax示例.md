---
layout: archives
title: Promise封装ajax示例
date: 2018-05-11 16:47:01
tags: es6 Promise
categories: es6
---
### Promise使用示例（ajax）
```
/*使用Promise封装ajax*/
function getList(url, data) {
	return new Promise(function (resolve, reject) {
		$.ajax({
			url: HanderServiceUrl+url,
			type: "POST",
			dataType: 'json',
			data: data,
			success: function (json) {
				resolve(json)
			},
			error: function (err) {
				reject(json)
			}
		})
	})
}
/*调用示例*/
/*es6*/
getList("/Eva_Manage/Eva_ManageHandler.ashx", {
	Func: "Get_Eva_Regular_A",
	StartTime: null,
	EndTime: null,
	StudentUID: null,
	TeacherUID: null,
	IsBack_Log: true,
	IncludeClass: false,
	ExpertUID: GetLoginUser().UniqueNo,
	IsCurrentSection: false,
	"SectionId": '',
	"sort_nav_edu": '',
	PageIndex: 1,
	PageSize: 3,
	CourseUID: '',
	Key: ''
}).then(function (json) {
	console.log(json)
})
/*es7*/
async function Get_Eva_Regular_Expert1(page) {
	var json = await getList("/Eva_Manage/Eva_ManageHandler.ashx", {
		Func: "Get_Eva_Regular_A",
		StartTime: null,
		EndTime: null,
		StudentUID: null,
		TeacherUID: null,
		IsBack_Log: true,
		IncludeClass: false,
		ExpertUID: GetLoginUser().UniqueNo,
		IsCurrentSection: false,
		"SectionId": '',
		"sort_nav_edu": '',
		PageIndex: 1,
		PageSize: 3,
		CourseUID: '',
		Key: ''
	})
	console.log(json)
}
```