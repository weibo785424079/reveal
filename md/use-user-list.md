* useUserList 封录状态 id => useList

```tsx
import React, { useState } from 'react';

const useUserList = (id) => {
    const [userList, setUserList] = useState([])

    useEffect(() => {
        fetch().then(res => setUserList(res))
    },[])

    return userList
}

const Demo = ({id}) => {
    const userList = useUserList(id);
};
```