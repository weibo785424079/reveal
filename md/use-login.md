* useLogin 封装登录状态

```tsx [5-15|17-28]
import React, { useState } from 'react';

const Index = () => <div>需要登录才能查看的页面</div>

const useLogin = () => {
    const [logined, setLogined] = useState(false)

    useEffect(() => {
        fetch().then(res => setLogin(res))
    },[])

    return logined
}

const Demo = () => useLogin() ? <Index/> : null;

const withLogin = WrappedComponent => class extends React.Component {
    state = {logined: false}

    componentDidMount() {
        fetch().then(res => this.state({logined: res}))
    }

    render() {
        return this.state.logined ? <WrappedComponent {...this.props}/> : null;
    }
}
const Demo = withLogin(Index)
```