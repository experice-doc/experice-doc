# 个人总结的一些开发规范

### controller:
    所有变量命名方式：驼峰 首字母小写后面的大写如：wayType
    方法：
    （1）公有的：获取信息的加get  驼峰 如：getPayInfo()
               添加信息的加add  驼峰 如：addPayInfo()
               修改信息的加 update   驼峰  如：updatePayInfo()
               删除信息的加  delete  驼峰   如：deletePayInfo()


    （2）私有的：方法前全部加__   
            如检查参数 __checkParams()
            受保护的：方法前全部加_
            如检查参数 _checkParams()


    （3）入参规范：不要一个一个穿 传array  
                如：getPayInfo([‘aid’=$aid,’womid’=>$womid])


                这样便于对参数验证。参数要在方法的上面加上注释。
                如：/*
                    *params:$params[‘aid’] int 商户id  *return:返回值格式以及返回什么东西
                    */


    （4）方法括号的规范：如
        function __checkParams($params)
        {
            方法体
        }
        不要 function __checkParams($params){
            方法体
        }
        写法的好处是 便于查看 如：多级if else 的时候便于查看


###  model的规范
        model里面尽量不要处理业务逻辑。只做对数据的操作。这样model比较干净
        方案1：业务处理全部放在controller层  
        方案2：加一层service，业务层只负责接收数据和对数据的校验，把所有的逻辑处理放在series层。由service层去和db打交道


###  db
        不要写裸sql，写裸sql就意味着sql漏洞
        尽量避免多表查询 性能会比较慢
        创建表 按照英文名称去建立字段名  所有的字段名加描述。


### 接口层规范
        每个接口一个类，以文件名去区分。
        每个接口2个主要方法
        一个是接口描述  描述里面是接受的所有参数的描述，包括（数据类型，验证规则，描述，举例）等。如：public function getParams()
            {
                $return['params'] = array(
                    'shop_id' => ['type'=>'int','valid'=>'required','description'=>'店铺id','example'=>'2','default'=>''],
                    'status' => ['type'=>'string','valid'=>'','description'=>'商品状态','example'=>'2','default'=>''],
                );
                return $return;
            }
        一个是主要的方法：如：
        public function itemCount($params)
        {
        return $res;
        }
        这样做的好处：
        1：接口分类。
        2：每个接口一个类，便于维护。
        3：如果有人中间走了 后面的人不至于读代码读的很费劲

### 没有一个号的开发规范，后面维护成本很高。
