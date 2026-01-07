# Purifier
> composer require mews/purifier

> use Mews\Purifier\Facades\Purifier;

> $cleanHtml = Purifier::clean($validated['html_content']);

## Làm sạch tự động trong Form Request (Gọn nhất)
```php
use Mews\Purifier\Facades\Purifier;

protected function passedValidation()
{
    // Ghi đè giá trị html_content bằng bản đã làm sạch
    $this->merge([
        'html_content' => Purifier::clean($this->html_content),
    ]);
}
```
### làm sach nếu có content
```php
// Chỉ làm sạch nếu html_content tồn tại, tránh ghi đè null vào dữ liệu
    if ($this->has('html_content')) {
        $this->merge([
            'html_content' => Purifier::clean($this->input('html_content')),
        ]);
    }
```