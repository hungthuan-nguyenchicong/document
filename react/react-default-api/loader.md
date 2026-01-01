# oaders/posts/postIndexLoader.jsx
```bash
// loaders/posts/postIndexLoader.jsx
export async function postIndexLoader() {
    const token = localStorage.getItem('laravel_token')
    const response = await fetch('/api/posts', {
        method: 'get',
        headers: {
            'Accept': 'application/json',
            'Authorization': 'Bearer ' + token
        }
    })
    if (response.ok) {
        return await response.json()
    }
    //console.log(response)
    throw new Response(response.statusText, { status: response.status });
}
```
# loaders/posts/postShowIdLoader.jsx
```bash
// loaders/posts/postShowIdLoader.jsx
export async function postShowIdLoader({ params }) {
    const id = params.id;
    const token = localStorage.getItem('laravel_token')
    const response = await fetch(`/api/posts/${id}`, {
        method: 'get',
        headers: {
            'Accept': 'application/json',
            'Authorization': 'Bearer ' + token
        }
    })
    if (response.ok) {
        return await response.json()
    }
    throw new Response(response.statusText, { status: response.status })
}
```