* useRequest给请求添加状态

```tsx [1-8|10-15]
import React, { useCallback } from 'react';
import { useRequest } from '@tms/site-hook';

const getUserList = useCallback((id) => {
    return fetch(id).then(userList => userList)
}, [id])

const { result, loading, error, run } = useRequest(getUserList);

const useUserList = (id) => {
	const getUserList = useCallback((id) => {
    	return fetch(id).then(userList => userList)
	}, [id])
	return useRequest(getUserList)
}; // id => userList,loading,error,run
```