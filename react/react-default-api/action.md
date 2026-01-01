# actions/posts/postStoreAction.jsx
```bash
import { redirect } from "react-router";

// actions/posts/postStoreAction.jsx
export async function postStoreAction({ request }) {
    const formData = await request.formData();
    const token = localStorage.getItem('laravel_token')
    // const title = formData.get('title')
    // console.log(formData)

    const response = await fetch('/api/posts', {
        method: 'post',
        headers: {
            'Accept': 'application/json',
            //'Authorization': 'Bearer ' + token
        },
        body: formData
    })
    if (response.ok) {
        throw redirect('/posts')
    }
    //return await response.json()
    throw new Response(response.statusText, { status: response.status });
}
```
# actions/posts/postEditAction.jsx
```bash
import { redirect } from "react-router";

// actions/posts/postEditAction.jsx
export async function postEditAction({ request, params }) {
    const id = params.id
    const formData = await request.formData();
    // Thêm dòng này để Laravel hiểu đây là PUT
    formData.append('_method', 'put');
    const token = localStorage.getItem('laravel_token')
    //console.log(formData.get('title'))
    //console.log(id)
    const response = await fetch(`/api/posts/${id}`, {
        method: 'post',
        headers: {
            'Accept': 'application/json',
            'Authorization': 'Bearer ' + token
        },
        body: formData
    })
    if (response.ok) {
        throw redirect('/posts')
    }
    return await response.json()
}
```
# actions/posts/postDestroyAcation.jsx
```bash
import { redirect } from "react-router"

// actions/posts/postDestroyAcation.jsx
export async function postDestroyAction({ params }) {
    const id = params.id
    const token = localStorage.getItem('laravel_token')
    //console.log(id)

    const response = await fetch(`/api/posts/${id}`, {
        method: 'POST',
        headers: {
            'X-HTTP-Method-Override': 'DELETE',
            'Accept': 'application/json',
            'Authorization': 'Bearer ' + token
        }
    })

    if (response.ok) {
        throw redirect('/posts')
    }
    //console.log(response)
    throw new Response(response.statusText, { status: response.status })


}
```