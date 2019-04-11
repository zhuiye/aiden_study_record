# 畅客管理后台入门手册

## 采用技术

Antd-design-pro 脚手架架构

https://pro.ant.design/docs/getting-started-cn

https://less.bootcss.com/#-variables-         less 中文网

## 介绍项目主要目录

```html
config
   config.js -------   该目录配置项目真实服务器的代理，还有路由的格式
   route.config.js  ----- 最主要，配置菜单和路由的格式
docker  ---这个目录暂时还不知道干嘛..
mock  ----改目录下存放假数据...用于前端的开发测试，自己定义一套返回
src
  assets ----放图片资源的目录
  components  ----改目录下定义常用的组件
  e2e     ------这个是测试的用例目录
  layouts ------页面的外围布局目录
     BasicLayout.js  这个文件最为重要，首页的布局，包括菜单栏，导航栏，面包屑导航
  locales
     zh-cn.js  语言本地化 定义 映射。。中文的映射
  models  ------该目录下存在的js文件，用来保存state,触发异步的action  更新state
  pages  -----页面的页面....
  services------ 此文件下存放着 向实际发送的api请求封装过程
  utils  ---- 工具类......
 
```

## 分析router.config.js

摘取项目部分代码如下:

```js
export default [
  // user
  { path: '/', redirect: '/blank' },  // 一开始匹配 '/ 跳转到app下面的,redirect 重定向'
    // 配置 关于用户登录页的路由
  {
    path: '/user',
    component: '../layouts/UserLayout',  // 这个相当一个页面的公共部分，把 Login 组件包裹着
    routes: [
      { path: '/user', redirect: '/user/login' }, // 如果路径是 /user 重定向到'/user/login' 
      { path: '/user/login', component: './User/Login' }, //渲染login组件
    ],
  },
  // app  ---这里配置菜单项和路由的映射
  {
    path: '/',
    component: '../layouts/BasicLayout',
    routes: [
      { path: '/blank', component: 'blank' },
      // 实名认证  ----菜单项的映射....locales cn-zh.js 下配置中文名
      {
        path: '/certification',
        name: 'certification',  //  'menu.certification': '实名认证',
        icon: 'usergroup-add',  //菜单的图标
        routes: [
          {
            path: '/certification/review',
            name: 'review',  //  审核资质 
            component: './Certification/Review',  //包裹组件
            routes: [
              {
                path: '/certification/review',  // 配置路径,因为审核资质页面有三个tab  
                redirect: '/certification/review/aut', // 重定向审核中
              },
              {
                path: '/certification/review/aut',
                component: './Certification/Review-aut', //审核中
              },
              {
                path: '/certification/review/pas',
                component: './Certification/Review-pas',  // 已通过
              },
              {
                path: '/certification/review/ret',  //已驳回
                component: './Certification/Review-ret',
              },
              {
                path: '/certification/review/review-detail/:infoId', // 详情掺入infoId参数 
                component: './Certification/Review-detail', //   详情
              },
            ],
          },
          // {
          //   path: '/certification/review-detail/:infoId',
          //   component: './Certification/Review-detail'
          // },
        ],
      },
      // 钱客通
      {
        path: '/qkt',
        name: 'qkt',
        icon: 'pay-circle-o',
        routes: [
          // 商户录入
          {
            path: '/qkt/entering',
            name: 'entering',
            icon: 'contacts',
            routes: [
              {
                path: '/qkt/entering/wait',
                name: 'wait',
                component: './Qkt-Entering/Wait',
              },
              {
                path: '/qkt/entering/wait/edit/:id',
                component: './Qkt-Entering/Wait-edit',
              },
              {
                path: '/qkt/entering/finish',
                name: 'finish',
                component: './Qkt-Entering/Finish',
              },
              {
                path: '/qkt/entering/finish/edit/:id',
                component: './Qkt-Entering/Finish-edit',
              },
            ],
          },
          // 机具管理
          {
            path: '/qkt/machine',
            name: 'machine',
            icon: 'tablet',
            routes: [
              {
                path: '/qkt/machine/enter',
                name: 'enter',
                component: './Qkt-Machine/Enter',
              },
              {
                path: '/qkt/machine/selfdevice/review',
                name: 'selfdevicereview',
                component: './Qkt-Machine/selfdevice-review',
              },
              {
                path: '/qkt/machine/selfdevice/review/detail/:type/:id',
                name: 'selfdevicereviewdetail',
                component: './Qkt-Machine/selfdevice-review-detail',
                hideInMenu: true,
              },
              {
                path: '/qkt/machine/selfdevice/manage',
                name: 'selfdevicemanage',
                component: './Qkt-Machine/selfdevice-manage',
              },
            ],
          },
          // 畅客管理
          {
            path: '/qkt/ckmanage',
            name: 'ckmanage',
            icon: 'contacts',
            component: './Qkt-CKmanage/ck',
          },
          {
            path: '/qkt/ckmanage/details/:id',
            name: 'details',
            hideInMenu: true,
            component: './Qkt-CKmanage/ck-details',
          },
          {
            path: '/qkt/ckmanage/jiju/:id',
            name: 'jiju',
            hideInMenu: true,
            component: './Qkt-CKmanage/ck-jiju',
          },
          {
            path: '/qkt/ckmanage/policy/:id',
            name: 'policy',
            hideInMenu: true,
            component: './Qkt-CKmanage/ck-policy',
          },
          {
            path: '/qkt/ckmanage/surplus/:id',
            name: 'surplus',
            hideInMenu: true,
            component: './Qkt-CKmanage/ck-surplus',
          },
          {
            path: '/qkt/ckmanage/switch',
            name: 'ck-switch',
            hideInMenu: true,
            component: './Qkt-CKmanage/ck-switch',
          },
          {
            path: '/qkt/ckmanage/switch/:id/auditing',
            component: './Qkt-CKmanage/switch-auditing',
          },
          {
            path: '/qkt/ckmanage/surplus/:id/record',
            name: 'record',
            hideInMenu: true,
            component: './Qkt-CKmanage/surplus-record',
          },
          {
            path: '/qkt/ckmanage/surplus/:id/record/check',
            name: 'check',
            hideInMenu: true,
            component: './Qkt-CKmanage/record-check',
          },
          {
            path: '/qkt/ckmanage/surplus/:id/switch',
            name: 'switch',
            hideInMenu: true,
            component: './Qkt-CKmanage/surplus-switch',
          },
        ],
      },

      {
        component: '404',
      },
    ],
  },
];

```

### 添加新页面路由格式如下

```js
{
    path: '/qkt/entering/wait',
    name: 'wait', 
    component: './Qkt-Entering/Wait',
},
```

## 分析登录 页面的流程

### UserLayout.js

```js
import React from 'react';
import styles from './UserLayout.less'; // 用的是cssModule

class UserLayout extends React.PureComponent {
  render() {
      //  这个children 也是下文的Login 组件
    const { children } = this.props;
    return (
      <div className={styles.container}>
        <div className={styles.content}>
          <div className={styles.top}>
            <div className={styles.header}>
              <span className={styles.title}>畅客通管理后台</span>
            </div>
          </div>
          {children}
        </div>
      </div>
    );
  }
}

export default UserLayout;

```



```js
import React, { Component } from 'react'; //
import { connect } from 'dva'; // connect  链接
import sha1 from '@/utils/hex_sha1';  // sha1工具  @ 代表src下的utils
import { formatMessage, FormattedMessage } from 'umi/locale';
import { Checkbox, Alert } from 'antd';
import Login from '@/components/Login';
import styles from './Login.less';

const { UserName, Password, Submit } = Login;

// 这个不清楚....
@connect(({ login, loading }) => ({
  login,
  submitting: loading.effects['login/login'],
}))
class LoginPage extends Component {
  state = {
    autoLogin: true,
  };


  // 处理提交.....
  handleSubmit = (err, values) => {
    if (!err) {
      const { dispatch } = this.props;
      dispatch({
        type: 'login/login',
        payload: {
          phone: values.phone,
          password: sha1(
            `9823"rhdAGF3'4T>%$@gadgu8@324${values.password}ASosrg89025r'ng^&@12-"~!$^%sbvuofsd${
              values.phone
            }`
          ),
        },
      });
    }
  };

  changeAutoLogin = e => {
    this.setState({
      autoLogin: e.target.checked,
    });
  };

  renderMessage = content => (
    <Alert style={{ marginBottom: 24 }} message={content} type="error" showIcon />
  );

  render() {
    const { login, submitting } = this.props;
    const { autoLogin } = this.state;
    return (
      <div className={styles.main}>
        <Login
          onSubmit={this.handleSubmit}
          ref={form => {
            this.loginForm = form;
          }}
        >
          <div style={{ marginTop: 20 }}>
            {login.status === 'error' &&
              !submitting &&
              this.renderMessage(formatMessage({ id: 'app.login.message-invalid-credentials' }))}
            <UserName name="phone" placeholder="请输入手机号" />
            <Password
              name="password"
              placeholder="请输入密码"
              onPressEnter={() => this.loginForm.validateFields(this.handleSubmit)}
            />
          </div>
          <div>
            <Checkbox checked={autoLogin} onChange={this.changeAutoLogin}>
              <FormattedMessage id="app.login.remember-me" />
            </Checkbox>
          </div>
          <Submit loading={submitting}>
            <FormattedMessage id="app.login.login" />
          </Submit>
        </Login>
      </div>
    );
  }
}

export default LoginPage;

```

### user/ Login.js

```js
import React, { Component } from 'react'; //
import { connect } from 'dva'; // connect  链接
import sha1 from '@/utils/hex_sha1';
import { formatMessage, FormattedMessage } from 'umi/locale'; //这两个函数，稍后解释
import { Checkbox, Alert } from 'antd';
import Login from '@/components/Login'; // antd-design-pro 封装的 Login 组件,直接拿来用就行
import styles from './Login.less';

const { UserName, Password, Submit } = Login; 

// 这个函数可以这么理解,connect就如同redux connect 一样...
// 注入state 转外为改组件的props...
@connect(({ login, loading }) => ({
  login, // 注入login state
  submitting: loading.effects['login/login'],
    /*
       提交中的loadding加载指示器...
       一开始是undefined,因为动作还没开始，动作开始之后就是ture,
       异步操作结束之后就变为 true 了，也就隐藏了起来.
    */
}))
class LoginPage extends Component {
  state = {
    autoLogin: true, //是否自动登录
  };

   
  // 处理提交.....
  handleSubmit = (err, values) => {
      // 如果erro为空,说明验证通过
      
      //values 是一个 json对象,
    if (!err) {
      const { dispatch } = this.props;
        // dispatch 派发动作 action 为 login/login  --此步操作，看models/login.js
        /*
          payload 传参格式....
        */
      dispatch({
        type: 'login/login',
        payload: {
          phone: values.phone,
            // 处理密码
          password: sha1(
            `9823"rhdAGF3'4T>%$@gadgu8@324${values.password}ASosrg89025r'ng^&@12-"~!$^%sbvuofsd${
              values.phone
            }`
          ),
        },
      });
    }
  };
  // 监听勾选自动登录的状态
  changeAutoLogin = e => {
    this.setState({
      autoLogin: e.target.checked,
    });
  };
  // 渲染错误 消息
  renderMessage = content => (
    <Alert style={{ marginBottom: 24 }} message={content} type="error" showIcon />
  );

  render() {
    const { login, submitting } = this.props;
    const { autoLogin } = this.state;
    return (
      <div className={styles.main}>
        <Login
          onSubmit={this.handleSubmit}
          ref={form => {
               // 获取表单对象loginForm
            this.loginForm = form;
          }}
        >
          <div style={{ marginTop: 20 }}>
            {login.status === 'error' &&
              !submitting &&
              this.renderMessage(formatMessage({ id: 'app.login.message-invalid-credentials' }))}
            <UserName name="phone" placeholder="请输入手机号" />
            <Password
              name="password"
              placeholder="请输入密码"
              onPressEnter={() => this.loginForm.validateFields(this.handleSubmit)}
            />
          </div>
          <div>
            <Checkbox checked={autoLogin} onChange={this.changeAutoLogin}>
                {/*自动登录*/}
              <FormattedMessage id="app.login.remember-me" />
            </Checkbox>
          </div>
         {/*
            loading=submitting,表示正在加载，一经触发，带处理完后恢复
         */}
          <Submit loading={submitting}>
              {/*这个是FormattedMessage格式化app.login.login 这个字符串在
               en-ch.js 中定义，且随我看
               'app.login.message-invalid-credentials': '账户或密码错误（admin/ant.design）',
               'app.login.message-invalid-verification-code': '验证码错误',
               'app.login.tab-login-credentials': '账户密码登录',
               'app.login.tab-login-mobile': '手机号登录',
               'app.login.remember-me': '自动登录',
               'app.login.forgot-password': '忘记密码',
               'app.login.sign-in-with': '其他登录方式',
               'app.login.signup': '注册账户',
               'app.login.login': '登录',
              */}
            <FormattedMessage id="app.login.login" />
          </Submit>
        </Login>
      </div>
    );
  }
}

export default LoginPage;

```

### models/login.js

```js
import { routerRedux } from 'dva/router'; // routerRedux 路由处理器
import { accountLogin, accountLogout } from '@/services/api'; //这个很重要主要触发request请求
import { notification } from 'antd'; //  notification 组件
/*
   好好分析一下model  下的js 文件的结构
   暴露出一个对象 namespace 的定义指待是关于那一块的state,有点相当于flux 里面多个store,但实质还是一个		store, 脚手架做了封装，参考redux 的合并reducer就知道了
*/
export default {
  namespace: 'login', // 命名空间为login

  state: {
    status: undefined,  // 状态的初始值为undefined
  },
   // effects 属性专门处理action 的操作，不管是异步的还是同步的
    /*
       同步---- 
       *add({ payload }, {  put }){
            yield put({
                type:'addCount',
                payload
            })
   		 },
    */
  effects: {
    *login({ payload }, { call, put }) {
        // call 调用请求,accountLogin 请求，
      const response = yield call(accountLogin, payload);
       // 返回的响应结果，通过put 更新state
      if (response) {
        // Login successfully 登录成功，保存本地....
        localStorage.setItem('globalUserName', JSON.stringify(payload.phone));
          //  执行路由替换...,调到/blank
        yield put(routerRedux.replace('/blank'));
          // 通知登录成功,弹出框
        notification.success({
          message: response.cnmsg,
        });
      }
    },
   // 登出处理......
    *logout(_, { call, put }) {
        //退出登录....
      const response = yield call(accountLogout);
        //设置本地globalUserName 为空
      localStorage.setItem('globalUserName', '');
      if (response) {
        yield put(
          // 执行这个函数---跳到'/user/login'路由
          routerRedux.push({
            pathname: '/user/login',
          })
        );
      }
    },
   
  },
  // 登录登出的比较简单，没有储存在state...
  reducers: {},
};

```

### sevices/api.js

仅仅摘取一部分

```js
import { stringify } from 'qs';
import request from '@/utils/request';

/* 
   遵照此等格式--- export async function accountLogin(){}
*/
export async function accountLogin(params) {
  return request('/admin/login', {
    method: 'POST',
    body: params,
  });
}

export async function accountLogout() {
  return request('/admin/logout', {
    method: 'POST',
    body: {},
  });
}

```

## 分析首页

### Layout/basicLayouts

```js
import React from 'react';
import { Layout, notification } from 'antd';
import DocumentTitle from 'react-document-title';
import isEqual from 'lodash/isEqual';
import memoizeOne from 'memoize-one'; // 可记忆化参数
import { connect } from 'dva';
import { ContainerQuery } from 'react-container-query';
import classNames from 'classnames';
import pathToRegexp from 'path-to-regexp';
import { enquireScreen, unenquireScreen } from 'enquire-js';
import { formatMessage } from 'umi/locale';
import SiderMenu from '@/components/SiderMenu';
import SettingDrawer from '@/components/SettingDrawer';
// import logo from '../assets/logo.svg';
import logo from '../assets/head_system.svg';
import Footer from './Footer';
import Header from './Header';
import Context from './MenuContext';

// 都把菜单的menu modal 放着了这里.....

const { Content } = Layout;
// const finalMenuData = [];

// Conversion router to menu. --转化路由成menu
function formatter(data, parentName) {
  return data
    .map(item => {
      if (!item.name || !item.path) {
        return null;
      }

      let locale = 'menu';
      if (parentName) {
        locale = `${parentName}.${item.name}`;
      } else {
        locale = `menu.${item.name}`;
      }

      const result = {
        ...item,
        name: formatMessage({ id: locale, defaultMessage: item.name }),
        locale,
      };
      if (item.routes) {
        const children = formatter(item.routes, locale);
        // Reduce memory usage
        result.children = children;
      }
      delete result.routes;
      return result;
    })
    .filter(item => item);
}
// 此函数的作用是吧router 映射成 menudata
const memoizeOneFormatter = memoizeOne(formatter, isEqual);

const query = {
  'screen-xs': {
    maxWidth: 575,
  },
  'screen-sm': {
    minWidth: 576,
    maxWidth: 767,
  },
  'screen-md': {
    minWidth: 768,
    maxWidth: 991,
  },
  'screen-lg': {
    minWidth: 992,
    maxWidth: 1199,
  },
  'screen-xl': {
    minWidth: 1200,
    maxWidth: 1599,
  },
  'screen-xxl': {
    minWidth: 1600,
  },
};

class BasicLayout extends React.PureComponent {
  constructor(props) {
    super(props);
    this.getPageTitle = memoizeOne(this.getPageTitle); // 获取标题
    this. getBreadcrumbNameMap = memoizeOne(this.getBreadcrumbNameMap, isEqual); //  获取map
      // 获取面包屑导航
    this.breadcrumbNameMap = this.getBreadcrumbNameMap(); //  获取breadcrumbNameMao
    this.matchParamsPath = memoizeOne(this.matchParamsPath, isEqual);
  }

  state = {
    rendering: true,
    isMobile: false,
      // 获取菜单
    menuData: this.getMenuData(),
  };

  componentDidMount() {
    const { dispatch } = this.props;
    // this.checkLoginStatus(); // 检查登陆状态
      // 获取设置......
    dispatch({
      type: 'setting/getSetting',
    });
      //  发展...
    if (process.env.NODE_ENV !== 'development') {
      // 如果不是开发环境获取授权数据...对的
      this.fetchAuthoritedMenuData();
    }
      // 一个动画--渲染false
    this.renderRef = requestAnimationFrame(() => {
      this.setState({
        rendering: false,
      });
    });
    // 查询是不是moble?
    this.enquireHandler = enquireScreen(mobile => {
      const { isMobile } = this.state;
      if (isMobile !== mobile) {
        this.setState({
          isMobile: mobile,
        });
      }
    });
  }

  componentDidUpdate(preProps) {
    // After changing to phone mode,
    // if collapsed is true, you need to click twice to display
      // 获取面包屑导航
    this.breadcrumbNameMap = this.getBreadcrumbNameMap();
    const { isMobile } = this.state;
    const { collapsed } = this.props;
    if (isMobile && !preProps.isMobile && !collapsed) {
      this.handleMenuCollapse(false);
    }
  }

  componentWillUnmount() {
    cancelAnimationFrame(this.renderRef);
    unenquireScreen(this.enquireHandler);
  }
  // 获取 上文下....
  getContext() {
    const { location } = this.props;
    return {
      location,
      breadcrumbNameMap: this.breadcrumbNameMap,
    };
  }

  // 获取菜单数据项

  getMenuData() {
    const {
      route: { routes },
    } = this.props;
    // 获取菜单项 ，静态的路由
    // console.log(memoizeOneFormatter(routes))
      // 获取菜单项
    return memoizeOneFormatter(routes);
  }

  /**
   * 获取面包屑映射
   * @param {Object} menuData 菜单配置
   */
  getBreadcrumbNameMap() {
    const routerMap = {};
    const mergeMenuAndRouter = data => {
      data.forEach(menuItem => {
        if (menuItem.children) {
          mergeMenuAndRouter(menuItem.children);
        }
        // Reduce memory usage
        routerMap[menuItem.path] = menuItem;
      });
    };
    mergeMenuAndRouter(this.getMenuData());
    return routerMap;
  }

  checkLoginStatus = () => {
    if (!localStorage.getItem('globalUserName')) {
      notification.error({
        message: '登录状态失效,请重新登录',
      });
      setTimeout(() => {
        window.location.href = 'user/login';
      }, 1500);
    }
  };
  // 获取匹配的路由.....
  matchParamsPath = pathname => {
    const pathKey = Object.keys(this.breadcrumbNameMap).find(key =>
      pathToRegexp(key).test(pathname)
    );
    return this.breadcrumbNameMap[pathKey];
  };

  // 获取title ---这里设置getPageTitle
  getPageTitle = pathname => {
    const currRouterData = this.matchParamsPath(pathname);
    console.log('getPageTitle',currRouterData)
    if (!currRouterData) {
      return '畅客伙伴管理后台';
    }
    const message = formatMessage({
      id: currRouterData.locale || currRouterData.name,
      defaultMessage: currRouterData.name,
    });
    // console.log(message)
    return `${message} - 畅客伙伴管理后台`;
  };

  getLayoutStyle = () => {
    const { isMobile } = this.state;
    const { fixSiderbar, collapsed, layout } = this.props;
    if (fixSiderbar && layout !== 'topmenu' && !isMobile) {
      return {
        paddingLeft: collapsed ? '80px' : '256px',
      };
    }
    return null;
  };

  getContentStyle = () => {
    const { fixedHeader } = this.props;
    return {
      margin: '24px 24px 0',
      paddingTop: fixedHeader ? 64 : 0,
    };
  };
  // 处理折叠 不理...

  handleMenuCollapse = collapsed => {
    const { dispatch } = this.props;
    dispatch({
      type: 'global/changeLayoutCollapsed',
      payload: collapsed,
    });
  };

  fetchAuthoritedMenuData = async () => {
    const { dispatch } = this.props;
    // 获取授权的菜单数据
    dispatch({
      type: 'user/fetchAuthoritedMenuData',
      payload: {},
    });
  };

  // filterMenuData = (menuData, authoritedMenuData) => {
  //   menuData.forEach(item => {
  //     authoritedMenuData.forEach(item2 => {
  //       // 过滤一级菜单
  //       if (item.locale === item2.front_key) {
  //         finalMenuData.push(item);
  //         if (item.children && !item2.path) {
  //           // 有子菜单的情况下递归过滤
  //           finalMenuData.forEach(item3 => {
  //             // 找到finalMenu中对应front_key的项,作为父节点传入
  //             if (item3.locale === item2.front_key) {
  //               // 修改菜单名称
  //               item3.name = item2.title;
  //               this.recusiveMenuData(item.children, item2.children, item3);
  //             }
  //           });
  //         }
  //       }
  //     });
  //   });
  // };

  // recusiveMenuData = (menuData, authoritedMenuData, parentNode) => {
  //   const tmpParentNode = JSON.parse(JSON.stringify(parentNode));
  //   // 清空原框架菜单的children,改为后端提供的children
  //   tmpParentNode.children = [];
  //   menuData.forEach(item => {
  //     authoritedMenuData.forEach(item2 => {
  //       if (item.locale === item2.front_key) {
  //         if (item.children && !item2.path) {
  //           parentNode.children.forEach(item3 => {
  //             if (item3.locale === item2.front_key) {
  //               item3.name = item2.title;
  //               this.recusiveMenuData(item.children, item2.children, item3);
  //             }
  //           });
  //         }
  //         tmpParentNode.children.push(item);
  //         parentNode.children = tmpParentNode.children;
  //       }
  //     });
  //   });
  // };

  renderSettingDrawer() {
    // Do not render SettingDrawer in production
    // unless it is deployed in preview.pro.ant.design as demo
    const { rendering } = this.state;
    if ((rendering || process.env.NODE_ENV === 'production') && APP_TYPE !== 'site') {
      return null;
    }
    return <SettingDrawer />;
  }

  render() {
    const {
      navTheme,
      layout: PropsLayout,
      children,
      location: { pathname },
      authoritedMenuData,
    } = this.props;
    // console.log(navTheme,PropsLayout,children,pathname,authoritedMenuData)
    const { isMobile, menuData } = this.state;
    // if (authoritedMenuData.length > 0) {
    //   finalMenuData = [];
    //   memoizeOne(
    //     this.filterMenuData(
    //       JSON.parse(JSON.stringify(menuData)), // 解除引用,防止修改原菜单
    //       JSON.parse(JSON.stringify(authoritedMenuData))
    //     )
    //   );
    // }
    const isTop = PropsLayout === 'topmenu';
    const layout = (
      <Layout>
        {isTop && !isMobile ? null : (
          <SiderMenu
            logo={logo}
            theme={navTheme}
            onCollapse={this.handleMenuCollapse}
            menuData={process.env.NODE_ENV !== 'development' ? authoritedMenuData : menuData}
            isMobile={isMobile}
            {...this.props}
          />
        )}
        <Layout
          style={{
            ...this.getLayoutStyle(),
            minHeight: '100vh',
          }}
        >
          <Header
            menuData={process.env.NODE_ENV !== 'development' ? authoritedMenuData : menuData}
            handleMenuCollapse={this.handleMenuCollapse}
            logo={logo}
            isMobile={isMobile}
            {...this.props}
          />
          <Content style={this.getContentStyle()}>{children}</Content>
          <Footer />
        </Layout>
      </Layout>
    );
    return (
      <React.Fragment>
        <DocumentTitle title={this.getPageTitle(pathname)}>
          <ContainerQuery query={query}>
            {params => (
              <Context.Provider value={this.getContext()}>
                <div className={classNames(params)}>{layout}</div>
              </Context.Provider>
            )}
          </ContainerQuery>
        </DocumentTitle>
        {this.renderSettingDrawer()}
      </React.Fragment>
    );
  }
}
// 把user ,global ,setting 的state注册进去 
export default connect(({ user, global, setting }) => ({
  collapsed: global.collapsed,
  layout: setting.layout,
  authoritedMenuData: user.authoritedMenuData,
  ...setting,
}))(BasicLayout);

```

该目录下的footer header 控制头部和尾部

### TopNavHeader

### GlobalHeader ---全局的header 

RightContent.js 有图标还有菜单项的选项处理

## 分析审核资质页面

```js
import React, { Component } from 'react'
import { connect } from 'dva';
import router from 'umi/router'; // 路由的控制 router.push() 推进一个路由
import { Card, Input, Row, Col, Form, Button, DatePicker } from 'antd';
// 页面外围头部包裹组价
import PageHeaderWrapper from '@/components/PageHeaderWrapper'; 

import utilsL from '@/utils/utils_l';

import styles from './Review.less';

const FormItem = Form.Item;  //  表单项
const { RangePicker } = DatePicker;  //时间范围选择器
//注入 review的store
@connect(({ review }) => ({
  review,
}))
// 此为更改附近类的行为即是为Review 类传入form对象
@Form.create()
class Review extends Component {
  // tab切换.....
  handleTabChange = key => {
    const { match } = this.props;
      // 切换界面内的路由
    switch (key) {
      case 'aut':
        router.push(`${match.url}/aut`);
        break;
      case 'pas':
        router.push(`${match.url}/pas`);
        break;
      case 'ret':
        router.push(`${match.url}/ret`);
        break;
      default:
        break;
    }
  };
 // 处理搜索
  handleSearch = (hrefType) => {
    const { dispatch, form } = this.props;
    // 获取表单控件的值
    const searchInfo = utilsL.getQuery_t(form.getFieldsValue());

    const values = {}
    //{pn: 1, pc: 10, st: "1552197247", en: "1554357247", search: "13782331012"}
    for (const prop in searchInfo) {
      values[prop] = searchInfo[prop] === undefined ? null : searchInfo[prop]
    }
    switch (hrefType) {
            // 根据当前的路由，才派发哪个action
      case 'aut':
        dispatch({
          type: 'review/verifing',
          payload: { ...values, pn: 1, pc: 10 },
        })
        break;
      case 'pas':
        dispatch({
          type: 'review/success',
          payload: { ...values, pn: 1, pc: 10 },
        })
        break;
      case 'ret':
        dispatch({
          type: 'review/failure',
          payload: { ...values, pn: 1, pc: 10 },
        })
        break;
      default:
        break;
    }
  };
 // 主要搜索....
  mainSearch(hrefType) {
    const {
      form: { getFieldDecorator, resetFields }, // 一个是表单修饰器，一个是重置表单的方法
    } = this.props;
    return (
      <Form layout="inline" className={styles.tableListForm}>
        <Row gutter={{ md: 8, lg: 24, xl: 48 }}>
          <Col md={8} sm={24}>
            <FormItem label="账户查询">
              {getFieldDecorator('search')(
                <Input placeholder="请输入商户真实姓名、手机号" />
              )}
            </FormItem>
          </Col>
          <Col md={8} sm={24}>
            <FormItem label="选择日期">
              {getFieldDecorator('date')(
                <RangePicker
                  style={{ width: '100%' }}
                  placeholder={['开始日期', '结束日期']}
                />
              )}
            </FormItem>
          </Col>
          <Col md={8} sm={24}>
            <span className={styles.submitButtons}>
              <Button type="primary" onClick={() => this.handleSearch(hrefType)}>
                查询
              </Button>
              <Button style={{ marginLeft: 8 }} onClick={() => resetFields()}>
                清空
              </Button>
            </span>
          </Col>
        </Row>
      </Form>
    );
  }

  render() {
    const tabList = [
      {
        key: 'aut',
        tab: '审核中',
      },
      {
        key: 'pas',
        tab: '已通过',
      },
      {
        key: 'ret',
        tab: '已驳回',
      },
    ];

  const { match, children, location } = this.props;
  const href = location.pathname
  const hrefType = href.slice(href.length - 3, href.length)
    return (
      <PageHeaderWrapper
        title='认证材料审核'
        content={this.mainSearch(hrefType)} {/*主要的内容*/}
        tabList={tabList}
        tabActiveKey={href.replace(`${match.path}/`, '')}
        onTabChange={this.handleTabChange}
      >
        <Card bordered={false} style={{minHeight: 500}}>
          {children}
        </Card>
      </PageHeaderWrapper>
    )
  }
}

export default Review;

```

```js
/*审核中.......*/
import React, { Component } from 'react'
import router from 'umi/router'
import { connect } from 'dva';
import ShareTable from '@/components/ShareTable' //组件封装的表格
import { Button } from 'antd'

// 这个玩意就是 把 state 注册到改组件中
@connect(({ review, loading }) => ({
  review,
  loading: loading.effects['review/verifing'], // 加载指示器... 如果那个用到就使用它
}))
// 来个基本摸清套路......
class Aut extends Component {
  /*
    // 配置表格---
  */
  columns = [
    {
      title: '用户账号',
      dataIndex: 'phone',  // 映射的返回字段的属性名
      key: 'phone',  // 唯一key
      align: 'center', // 列居中显示
    },
    {
      title: '真实姓名',
      dataIndex: 'name',
      key: 'name',
      align: 'center',
    },
    {
      title: '提交时间',
      dataIndex: 'time',
      key: 'time',
      align: 'center',
    },
    {
      title: '直推上级',
      dataIndex: 'introducer',
      key: 'introducer',
      align: 'center',
    },
    {
      title: '注册应用',
      dataIndex: 'app',
      key: 'app',
      align: 'center',
    },
    {
      title: '操作',
      dataIndex: 'actions',
      key: 'actions',
      align: 'center',
      render: (text, record) => (
        <Button
          size="small"
          onClick={() => router.push(`/certification/review/review-detail/${record.id}`)}
        >
          资质审核
        </Button>
      )
    },
  ];

  componentDidMount() {
    const { dispatch } = this.props;
    // 获取数据
    dispatch({
      type: 'review/verifing',
      payload: { pn: 1, pc: 10 },
    })
  }

  handleStandardTableChange = pagination => {
    // 表格分页变化更改
    const { dispatch } = this.props;
    const params = {
      pn: pagination.current,   // 有意思，现在的页面 如点2 的话，也会2
      pc: pagination.pageSize,  //  一页的条数
    };

    dispatch({
      type: 'review/verifing',
      payload: params,
    });
  };

  render () {
    const { review: { data_v }, loading } = this.props
    return (
      <ShareTable
        columns={this.columns}
        rowKey={record => record.id}
        data={data_v}
        loading={loading}
        onChange={this.handleStandardTableChange}
        style={{minHeight: 500}}
      />
    )
  }
}
export default Aut

```

### models/review.js

```js
import {
  Verifing,
  Success,
  Failure,
  Detail,
  Pass,
  Refuse,
} from '@/services/certification'
import { message } from 'antd'

export default {
  namespace: 'review',  //命名空间为review

  state: {
    data_v: [], // 数据
    data_s: [], 
    data_f: [],
    auth_info: {},
    material: [],
    record_info: [],
  },

  effects: {
    *verifing({ payload }, { call, put }) {
        /*
           call方法调用 真正的异步请求，
           put方法，如果传入一个action  ,则更新相对应的state....
        */
        // 获取验证的数据
      const response = yield call(Verifing, payload);
      if (response) {
        yield put({
          type: 'saveVerifing',
          payload: response.data,
        });
      }
    },
    *success({ payload }, { call, put }) {
      const response = yield call(Success, payload);
      if (response) {
        yield put({
          type: 'saveSuccess',
          payload: response.data,
        });
      }
    },
    *failure({ payload }, { call, put }) {
      const response = yield call(Failure, payload);
      if (response) {
        yield put({
          type: 'saveFailure',
          payload: response.data,
        });
      }
    },
    *detail({ payload }, { call, put }) {
        
      const response = yield call(Detail, payload);
      const data = response && response.data;
      if (response && response.data && Object.keys(data).length !== 0 ) {
          
        yield put({
          type: 'saveDetail',
          auth_info: data.auth_info,
          material: data.material,
          record_info: data.record_info,
        });
      } else {
        yield put({
          type: 'saveDetail',
          auth_info: {},
          material: [],
          record_info: [],
        });
      }
    },
    *refuse({ payload }, { call, put }) {
      const response = yield call(Refuse, payload);
      if (response) {
        message.success(response.cnmsg)
      }
    },
    *pass({ payload }, { call, put }) {
      const response = yield call(Pass, payload);
      if (response) {
        message.success(response.cnmsg)
      }
    },
  },

  reducers: {
      // 更新数据.....
    saveVerifing(state, action) {
      return {
        ...state,
        data_v: action.payload,
      }
    },
    saveSuccess(state, action) {
      return {
        ...state,
        data_s: action.payload,
      }
    },
    saveFailure(state, action) {
      return {
        ...state,
        data_f: action.payload,
      }
    },
    saveDetail(state, action) {
      return {
        ...state,
        auth_info: action.auth_info,
        material: action.material,
        record_info: action.record_info,
      }
    },
  }
}

```

## 详情页面分析

```js
import React, { Component, Fragment } from 'react'
import { connect } from 'dva';
import {
  Card,
  Divider,
  List,
  Timeline,
  Button,
  Modal,
  Input,
  Form,
  Popconfirm,
  Icon,
 } from 'antd'
// import PageHeaderWrapper from '@/components/PageHeaderWrapper';
import DescriptionList from '@/components/DescriptionList';

import ImgModal from '@/components/ImgModal/index.tsx';

import styles from './Review-detail.less'

const FormItem = Form.Item;
const { Description } = DescriptionList;
const { TextArea } = Input

@connect(({ review, loading }) => ({
  review,
  loading: loading.effects['review/detail'], //正在加载...
}))
@Form.create()
class ReviewDetail extends Component {
  constructor(props) {
    super(props)
    this.state = {
      visible: false,
      phoneFlag: false,
      id: null,
      auth_info: {},
      material: [],
      record_info: [],
    }
  }

  componentDidMount = async() => {
    const { dispatch, match } = this.props;
    const id = match.params.infoId;  // infoId = 多少....

    await dispatch({
      type: 'review/detail',
      payload: { id },
    });
      //去获取数据,然后,得到数据..本地更新..
    const {
      review: { auth_info, material, record_info },
    } = this.props;
    this.setState({
      id,
      auth_info,
      material,
      record_info,
    })
  }

  handlePhone = (flag, index = null) => {
      // 当前的imgIndex
    this.setState({
      phoneFlag: flag,
      imgIndex: index,
    })
  }
 // 是否展现modal
  handleModal = flag => {
    this.setState(() => ({
      visible: flag,
    }))
  }
 
  // 拒绝
  handleRet = async() => {
    const { dispatch, form } = this.props;
    const { id } = this.state
    const searchInfo = form.getFieldsValue()
    await dispatch({
      type: 'review/refuse',
      payload: { id, reason: searchInfo.reason },
    })
    this.handleModal(false)
    await dispatch({
      type: 'review/detail',
      payload: { id },
    });
  }
  // 通过
  async handleOk() {
    const { dispatch } = this.props;
    const { id } = this.state
    await dispatch({
      type: 'review/pass',
      payload: { id },
    })
    await dispatch({
      type: 'review/detail',
      payload: { id },
    });
  }

  render () {
    const { visible, phoneFlag, imgIndex, auth_info, material, record_info } = this.state
    const {
      // review: { auth_info, material, record_info },
      form,
      loading,
    } = this.props
    const imageArr = []
    for(let v of material.values()) {
      imageArr.push(v.url)
    }

    const cardList = material ? (
      <List
        rowKey="id"
        loading={loading}
        grid={{ gutter: 24, xl: 4, lg: 3, md: 3, sm: 2, xs: 1 }}
        dataSource={material}
        renderItem={(item, index) => (
          <List.Item>
            <Card
              className={styles.card}
              hoverable
              cover={
                <div
                  style={{
                    width: '100%',
                    height: '222px',
                    backgroundImage: `url(${item.url})`,
                    backgroundSize: 'contain',
                    backgroundRepeat: 'no-repeat',
                    backgroundPositionX: 'center',
                    backgroundPositionY: 'center',
                    borderBottom: '1px solid #e8e8e8',
                  }}
                />
              }
              onClick={() => this.handlePhone(true, index)}
            >
              <Card.Meta
                title={<a>{item.type_name}</a>}
                style={{textAlign: 'center'}}
              />
            </Card>
          </List.Item>
        )}
      />
    ) : null;
    return (
      <Fragment>
        <Card bordered={false}>
          <DescriptionList
            size="large"
          >
            <Description term="商户账号">{auth_info.phone}</Description>
            <Description term="当前认证进度">{auth_info.status_name}</Description>
            <Description>&nbsp;</Description>
            <Description term="真实姓名">{auth_info.id_name}</Description>
            <Description term="身份证号码">{auth_info.id_number}</Description>
            <Description term="身份证有效期">{auth_info.validity_period}</Description>
            <Description term="银行卡号">{auth_info.card_number}</Description>
            <Description term="所属银行">{auth_info.bank_name}</Description>
            <Description term="银行预留手机号">{auth_info.card_phone}</Description>
            <Description term="活体检测结果">{auth_info.living_result}</Description>
          </DescriptionList>
          <Divider style={{ marginBottom: 32 }} />
          <DescriptionList
            size="large"
            title="图片材料"
          >
            <div className={styles.cardList}>{cardList}</div>
          </DescriptionList>
          <div className={styles.title}>审核纪录</div>
          <Timeline>
            {
              record_info && record_info.map((e, i) => (
                <div
                  style={{
                    display: 'flex',
                  }}
                  key={i}
                >
                  <div>{e.time}</div>
                  <Timeline.Item style={{margin: '6px 0 0 4px'}}/>
                  <div>
                    {e.type === 'fail' ?
                      <p>
                        <span
                          className={styles.failure}
                          onClick={() => console.log(123)}
                        >
                          驳回
                        </span>
                        {e.text}
                      </p> :
                      <p>{e.text}</p>
                    }
                    {e.type === 'fail' && <p>{ '驳回原因:' + e.sub_text }</p>}
                  </div>
                </div>
              ))
            }
          </Timeline>

          {auth_info.status === 2 &&
            <Fragment>
              <Popconfirm
                icon={<Icon type="question-circle-o" />}
                title='是否通过认证材料审核?'
                onConfirm={() => this.handleOk()}
              >
                <Button type='primary'>通过认证材料审核</Button>
              </Popconfirm>
              <Button
                type='danger'
                onClick={() => this.handleModal(true)}
                style={{
                  marginLeft: 10
                }}
              >
                审核不通过
              </Button>
            </Fragment>
          }

        </Card>
        <Modal
          title='不通过原因'
          visible={visible}
          onCancel={() => this.handleModal(false)}
          onOk={this.handleRet}
        >
          <FormItem>
            {form.getFieldDecorator('reason', {
            })(<TextArea autosize={{minRows: 4, maxRows: 6}} />)}
          </FormItem>
        </Modal>

        <ImgModal
          visible={phoneFlag}
          modalTitle="图片详情"
          imgIndex={imgIndex}
          onCancel={() => this.handlePhone(false)}
          imgSourceArray={imageArr}
        />

      </Fragment>
    )
  }
}
export default ReviewDetail;

```

## ImgModal组件分析

```js
import React from 'react';
import { Icon, Modal } from 'antd';
// 原来自己封装的ImgMoadl

const styles = require('./styles.less');
// 定义接口props 传经来props接口
interface IProps {
  visible: boolean; // 布尔值
  modalTitle?: string; //可选字符串类型
  onCancel: () => void; // 函数
  imgIndex: number;  // 数字
  imgSourceArray: string[]; //字符串数组
}
// typeScript 定义 state 中的类型 
interface IState {
  visible: boolean;  // 布尔型
  imgSourceArray: string[]; //字符串数组
  imgIndex: number; //数组
}

// <IProps,IState>
export default class ImgModal extends React.Component<IProps, IState> {
    // 相当于coponentWillRecevicerProps函数
  static getDerivedStateFromProps(nextProps, prevState) {
      // 如果父级变true,先前的为false
    if (nextProps.visible === true && prevState.visible === false) {
      return {
        visible: true, //模态框显现
        imgIndex: nextProps.imgIndex, //获取图片的下一个imgeIndex
      };
    }
      // 模态框隐藏
    if (nextProps.visible === false && prevState.visible === true) {
      return {
        visible: false,
      };
    }
      // 否则 返null
    return null;
  }
  // state 的初始化
  state: IState = {
    visible: false,  //模态框是否可见
    imgSourceArray: this.props.imgSourceArray, //从属性那得来的图片数组
    imgIndex: this.props.imgIndex, //当前的图片的 index
  };
  //上一个
  prevPic = () => {
    this.setState(prevStae => {
      if (prevStae.imgIndex > 0) {
        return {
          imgIndex: prevStae.imgIndex - 1,
        };
      } else {
        return {
          imgIndex: this.props.imgSourceArray.length - 1,
        }
      }
      // return {
      //   imgIndex: 0,
      // };
    });
  };
  //下一张图片
  nextPic = () => {
    this.setState(prevStae => {
      if (prevStae.imgIndex === this.props.imgSourceArray.length - 1) {
        return {
          // imgIndex: prevStae.imgIndex,
          imgIndex: 0,
        };
      } else {
        return {
          imgIndex: prevStae.imgIndex + 1,
        };
      }
    });
  };

  render() {
    const { visible, imgIndex } = this.state;
    const { modalTitle, onCancel, imgSourceArray } = this.props;
    return (
      <div >
        <Modal
          footer={null}
          title={modalTitle}
          visible={visible}
          onCancel={onCancel}
          className={styles.photoModal}
        >
          <div className={styles.imgWrap}>
            <img alt="example" src={imgSourceArray[imgIndex]} className={styles.img} />
            <div className={styles.arrowWrap}>
              <Icon
                type="left-circle"
                theme="filled"
                className={styles.photo_icon}
                onClick={this.prevPic}
              />
              <Icon
                type="right-circle"
                theme="filled"
                className={styles.photo_icon}
                onClick={this.nextPic}
              />
            </div>
          </div>
        </Modal>
      </div>
    );
  }
}

```

## 商家录入页面分析

```js
import React, { Component } from 'react'
import { connect } from 'dva';
import router from 'umi/router';
import PageHeaderWrapper from '@/components/PageHeaderWrapper'; //头部
import ShareTable from '@/components/ShareTable'  //表格列表
import { Form,
  Row,
  Col,
  Input,
  Card,
  Button,
 } from 'antd'

import styles from './index.less';

const FormItem = Form.Item;

@connect(({ entering, loading }) => ({
  entering,
  loading: loading.effects['entering/getPayee'],
}))
@Form.create()

export default class Wait extends Component {
  columns = [
    {
      title: '商户编号',
      dataIndex: 'merchant_no', //映射merchant_no
      key: 'merchant_no', //键值
      align: 'center', //居中显示
    },
    {
      title: '商户名称',
      dataIndex: 'merchant_name',
      key: 'merchant_name',
      align: 'center',
    },
    {
      title: '商户手机号',
      dataIndex: 'mobile',
      key: 'mobile',
      align: 'center',
    },
    {
      title: '机具型号',
      dataIndex: 'model',
      key: 'model',
      align: 'center',
    },
    {
      title: '终端号',
      dataIndex: 'terminal_id',
      key: 'terminal_id',
      align: 'center',
    },
    {
      title: 'SN码',
      dataIndex: 'sn',
      key: 'sn',
      align: 'center',
    },
    {
      title: '上级代理商',
      dataIndex: 'belong_user',
      key: 'belong_user',
      align: 'center',
      render: (text) => (
         {/*直接插入html*/} 
        <p dangerouslySetInnerHTML={{__html: text}}></p>
      ),
    },
    {
      title: '创建时间',
      dataIndex: 'create_time',
      key: 'create_time',
      align: 'center',
    },
    {
      title: '操作',
      dataIndex: 'actions',
      key: 'actions',
      align: 'center',
      render: (text, record) => (
        <Button
          size="small"
          onClick={() => router.push(`/qkt/entering/wait/edit/${record.id}`)}
        >
          编辑
        </Button>
      )
    },
  ];

  componentDidMount(){
    const { dispatch } = this.props;
      //派发异步action....
    dispatch({
      type: 'entering/getPayee',
      payload: { pn: 1, pc: 10, status: 'unprocessed' },
    })
  }
 // 处理表格的改变
  handleStandardTableChange = pagination => {
    const { dispatch } = this.props;
    const params = {
      pn: pagination.current,
      pc: pagination.pageSize,
      status: 'unprocessed',
    };
    dispatch({
      type: 'entering/getPayee',
      payload: params,
    });
  };

  handleSearch = () => {
      //直接拿来用就行了
    const { dispatch, form } = this.props;
    const searchInfo = form.getFieldsValue();
    const values = {}
    for (const prop in searchInfo) {
      values[prop] = searchInfo[prop] === undefined ? null : searchInfo[prop]
    }
    dispatch({
      type: 'entering/getPayee',
      payload: { ...values, pn: 1, pc: 10, status: 'unprocessed' },
    })
  }
 // 重置
  handleFormReset = () => {
    const { form } = this.props;
    // 重置，要熟悉 ant-design 那一套啊....
    form.resetFields();
  };

  // 搜索表单......
  SearchForm() {
    const {
      form: { getFieldDecorator },
    } = this.props;
    return (
        {/**行表单 。。。**/}
      <Form layout="inline">
        <Row gutter={{ md: 8, lg: 24, xl: 48 }}>
          <Col md={6} sm={24}>
            <FormItem label="商户编号">
              {getFieldDecorator('merchant_no')(<Input placeholder="请输入商户编号" />)}
            </FormItem>
          </Col>
          <Col md={6} sm={24}>
            <FormItem label="商户名称">
              {getFieldDecorator('merchant_name')(<Input placeholder="请输入商户名称" />)}
            </FormItem>
          </Col>
          <Col md={7} sm={24}>
            <FormItem label="机具编号">
              {getFieldDecorator('device_no')(<Input placeholder="请输入机具SN码或终端号" />)}
            </FormItem>
          </Col>
          <Col md={5} sm={24}>
            <span className={styles.submitButtons}>
              <Button type="primary" onClick={this.handleSearch}>
                查询
              </Button>
              <Button style={{ marginLeft: 8 }} onClick={this.handleFormReset}>
                重置
              </Button>
            </span>
          </Col>
        </Row>
      </Form>
    );
  }

  render() {
    const { entering: { data }, loading } = this.props;
    return (
      <PageHeaderWrapper title='待录入商户'>
        <Card bordered={false} style={{minHeight: 500}}>
          <div className={styles.tableList}>
            <div className={styles.tableListForm}>{this.SearchForm()}</div>
            <ShareTable
              columns={this.columns}
              data={data}
              rowKey={record => record.id}
              loading={loading}
              onChange={this.handleStandardTableChange}
              style={{minHeight: 500}}
            />
          </div>
        </Card>
      </PageHeaderWrapper>
    )
  }
}

```

