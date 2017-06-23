## rs-util

# 接入新的业务
(a) spc-common中添加2字段
1. ProductTypeCode  业务Code，每个业务1种code
2. PromoTypeCode

(b) bm-entity中添加2字段
3.1 ActivityTypeCode
3.2 ActivitySubTypeCode

(c) 更改spc-management-service.couponGroupTypeStr

* promoTypCode到支付中心CouponType映射
* 3 商家券
* 4 商家会员卡券
* defaultValue 3
* 用默认值即可

(c) 更改spc-management-service.couponGroupTypeStr
* promoTypeCode到支付中心收银台映射
* 1  团购
* 11 闪惠
* 22 KTV预订
* defaultValue 11

(d) spc-process-service.notifyTypeStr
* promoTypeCode到发系统消息映射
* -1, "不通知"
* 0, "系统消息和PUSH"
* 1,"系统消息"


