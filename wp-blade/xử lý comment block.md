```blade
{{-- @dd($p)

@dd($p->post_content) --}}
@php
    $pattern = '#<' . '!--.*?--' . '>#s';
    $clean_html = preg_replace($pattern, '', $p->post_content);
@endphp
<h1>{{ $p->post_title }}</h1>
<div>{!! $clean_html !!}</div>
```
