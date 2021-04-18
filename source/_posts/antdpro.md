---

title: antdpro
date: 2021-03-31 22:51:53
tags:
---

## 创建antdpro项目

```
yarn create umi pages
```



## 复刻资源管理

```
新建文件夹task/index.jsx
```

```
在config/config.js 添加 {path: '/system/task', component: './system/task'},
```

```
在资源管理页面 添加菜单资源 
```

```
渲染面包屑菜单栏
	return (
    <PageHeaderWrapper>
     </PageHeaderWrapper>
  )
```

```
渲染标签页

import {Tabs,} from 'antd'; 引入标签页 
const {TabPane} = Tabs; 重命名标签页
<TabPane tab={item.text} key={item.value}> </TabPane> 渲染标签页

链接，用于后端请求数据
let api = '/api/sys/resource/';
const tabsUrl = api + 'clientTypeOptions';

定义存放标签页的容器
const [tabsList, setTabsList] = useState([]);

页面渲染完成后就会执行useEffect 进而执行initTabs() 参数[] 即 初始化一次
useEffect(() => {
    initTabs();
  }, []);
 
给标签页容器赋值 通过get方法 请求后端数据 
const initTabs = () => {
    get(tabsUrl).then(rs => {
      setTabsList(rs);
    });
  };
```

```
渲染菜单列

页面渲染
<Col flex="350px">
   <Card bordered={false}>
    {resourceTree && resourceTree.length > 0 && <Tree treeData={resourceTree} />}
    </Card>
</Col>

定义数据容器 以及获取数据方法
const [resourceTree, setResourceTree] = useState([]);

数据获取方法入口
loadTree('', rs[0].value)

数据获取方法本体
const loadTree = function (defaultSelectedKey, tabKey) {
    get(treeUrl, {clientType: tabKey}).then(tree => {
      setResourceTree(tree);
    });
  };
```

```
渲染表单

定义表单
const [form] = Form.useForm();

页面渲染
                <Form form={form} labelCol={{flex: '120px'}} wrapperCol={{flex: 'auto'}}>
                  <Form.Item label="类型" name="type" rules={[{required: true, message: '请输入名称'}]}>
                    <Select>
                      <Select.Option value="FOLDER">目录</Select.Option>
                      <Select.Option value="MENU">菜单</Select.Option>
                      {tabKey !== 'APP' &&
                      <Select.Option value="FUNCTION">功能</Select.Option>
                      }
                    </Select>
                  </Form.Item>
                  {formValueType === 'FUNCTION' && <Form.Item label="权限码" name="code">
                    <Input/>
                  </Form.Item>}
                  {tabKey === 'APP' &&
                  <>
                    <Form.Item label="权限码" name="code">
                      <Input/>
                    </Form.Item>
                    <Form.Item label="显示系统" name="showSystem">
                      <RemoteSelect url={api + "showSystemOptions"}/>
                    </Form.Item>
                  </>
                  }
                  {formValueType === 'MENU' && tabKey !== 'APP' &&
                  <Form.Item label="页面路径" name="path"><Input/></Form.Item>}
                  <Form.Item label="名称" name="name" rules={[{required: true, message: '请输入名称'}]}>
                    <Input/>
                  </Form.Item>

                  <Form.Item name="pid" label="父节点">
                    <RemoteTreeSelect url={pidTreeUrl + "?clientType=" + tabKey}
                    />
                  </Form.Item>
                  <Form.Item label="显示顺序" name="seq">
                    <InputNumber/>
                  </Form.Item>

                  <Form.Item label="是否停用" name="disabled" valuePropName="checked">
                    <Checkbox/>
                  </Form.Item>

                  <Form.Item label="运维商权限" name="isMerchantRight" valuePropName="checked">
                    <Checkbox/>
                  </Form.Item>

                  <div style={{display: "flex", alignItems: "center", justifyContent: "flex-end", marginBottom: "8px"}}>
                    <div style={{marginRight: "45%", marginTop: "30px"}}>
                      <Button type="primary" htmlType="submit"> &nbsp;&nbsp;保&nbsp;存&nbsp;&nbsp; </Button>
                    </div>
                  </div>
                </Form>
                
表单是否可编辑                 
const [formDisabled, setFormDisabled] = useState(true);               
disabled={formDisabled}

用来控制某些字段是否可以显示
const [formValueType, setFormValueType] = useState('');
{formValueType === 'MENU' && tabKey !== 'APP' }

是否显示form表单
const [showForm, setShowForm] = useState(false);
className={showForm ? '' : 'hide'}
```

```
选中首个树节点

调用loadTree('',"PC")
const initTabs = () => {
    get(tabsUrl).then(rs => {
      setTabsList(rs);
      if (rs.length > 0) {      // 设置第一个单位为选中状态
        setTabKey(rs[0].value);
        loadTree('', rs[0].value)  //rs[0].value="pc"
      }
    });
  };
 
调用loadTree('',"PC") 
const loadTree = function (defaultSelectedKey, tabKey) {
		//获取tree
    get(treeUrl, {clientType: tabKey}).then(tree => {
    	//赋值
      setResourceTree(tree);
      //defaultSelectedKey有值就设置SelectedKeys
      if (defaultSelectedKey) {
        setSelectedKeys([defaultSelectedKey])
      } else if (tree.length > 0) {      // 设置第一个单位为选中状态
      	//tree有值就设置首节点
        if (tree && tree.length > 0) {
          const first = tree[0];
          const k = first.key;
          onSelect([k], {node: first})
        }
      } else {
        onSelect(null, {})
      }
    });
  };  
  
const onSelect = async (keys, {node}) => {
    setSelectedKeys(keys);
    if (keys == null || keys.length === 0) {
      setCurrentNode({});
      form.setFieldsValue({});
      form.resetFields();
      return
    }
    setFormDisabled(true);// 表单默认不可编辑状态
    setShowForm(true);  //展示表单
    let data = await handleGet(api + 'get', {id: keys});  //获取数据
    setCurrentNode(data);  //设置数据
    setFormValueType(data.type);  //设置数据
    form.resetFields();  //重置表单
    form.setFieldsValue(data);  //赋值表单
  };  
```

```
标签页变换事件

触发入口
onChange={clickTabs}
<Tabs defaultActiveKey={tabKey} onChange={clickTabs}  type="card" size={"small"}>

const clickTabs = (tabKey) => {
    setTabKey(tabKey);
    loadTree(null,tabKey)
  };
```

---



## 复刻员工管理

```
联级搜索
	1⃣️useEffect 调用relationSelect方法 根据当前用户 初始化数据 unit dept post
	2⃣️选择单位 触发 selectUnit  设置 selectValue.unit  调用findChildBy 传入参数type=dept
当selectValue.dept未设置时 就会调用screenTree 传入参数type=post
	3⃣️就会调用screenTree 回调用findChildBy方法 将会把deptList	
```



## 复显数据

```
let apiUser = '/api/currentUser';
获取数据直接初始化到form表单里面
get(apiUser).then(value=>{
	if("user" === value.userType){
		setFromValues({returneeId:value.id,returnDeptId:value.dept});
		form.setFieldsValue({returneeId:value.id,returnDeptId:value.dept});
	}
})
```

