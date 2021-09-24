* hook VS render-props
* useUserList 通过id获取人员列表 id => useList

```tsx [3-15|17-35]
import React, { useState } from 'react';

const useUserList = (id) => {
    const [userList, setUserList] = useState([])

    useEffect(() => {
        fetch().then(res => setUserList(res))
    },[])

    return userList
}

const Demo = ({ id }) => {
    const userList = useUserList(id);
};

class UserListWrap extends React.Component{
    state = {
        userList: []
    }

    componentDidMount() {
        fetch().then(userList => this.setState({userList}))
    }

    render() {
        return this.props.children(this.state.userList)
    }
}

const Demo = (id) = (
    <UserListWrap id={id}>
        {(userList) => <Component userList={userList} />}
    </UserListWrap>
)

```
* render-props嵌套，打乱代码结构,难以阅读
* 声明多余中间函数

