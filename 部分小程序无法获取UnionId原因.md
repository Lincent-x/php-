问题背景

通过观察数据，发现有一部分用户是无法获取到UnionId的
也就是接口返回的参数中不包含UnionId参数
看了微信文档的解释，只要小程序在开放平台绑定，就一定会分配UnionId
网上也有用户遇到这样的情况，没有解决
问题影响

使用微信小程序成功授权以后，下次在公众号中授权或在App中使用微信授权，无法识别是同一个微信用户，可能会出现一个微信用户绑定不同App用户的情况。

UnionID机制

微信对UnionId机制的原文解释

如果开发者拥有多个移动应用、网站应用、和公众帐号（包括小程序），可通过unionid来区分用户的唯一性，因为只要是同一个微信开放平台帐号下的移动应用、网站应用和公众帐号（包括小程序），用户的unionid是唯一的。换句话说，同一用户，对同一个微信开放平台下的不同应用，unionid是相同的。

同一个微信开放平台下的相同主体的App、公众号、小程序，如果用户已经关注公众号，或者曾经登录过App或公众号，则用户打开小程序时，开发者可以直接通过wx.login获取到该用户UnionID，无须用户再次授权。

注意： 后边这句话的描述

用户关注过公众号，或者曾经登录过App或公众号，则用户打开小程序时，开发者可以直接通过wx.login获取到该用户UnionID

即：如果用户没有关注过公众号，或者没有登陆过App，通过wx.login是无法获取到该用户UnionID，只能通过wx.getUserInfo来获取UnionId

经验证，系统不存在UnionId的小程序用户都是没有关注公众号或未在App中使用微信授权的用户

解决方案

获取小程序UnionId应该以wx.getUserInfo的UnionId为主
wx.getUserInfo需要用户授权，产品方面，需要考虑用户拒绝授权的处理流程