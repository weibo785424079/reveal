* hook的类生命周期及事件能力 useDocumentShow页面被唤醒执行副作用
* 对比类组件逻辑分散
  
```tsx [1-10|12-27]
import React, { useState } from 'react';
import { useDocumentShow } from '@tms/site-hook';

const Demo = () => {
  useDocumentShow(() => {
    alert('页面被唤醒了，调接口刷新数据');
  });

  return null;
};

class Demo extends React.Component {
    checkVisible = () => {
        if (document.visibilityState === 'visible') {
            this.props.onChange()
        }
    }
    componentDidMount () {
        document.addEventListener('visibilitychange', this.checkVisible);
    }
    componentWillUnmount() {
        document.removeEventListener('visibilitychange', this.checkVisible)        
    }
    render() {
        return '如果不使用HOC，代码就会混入业务逻辑中'
    }
}

```