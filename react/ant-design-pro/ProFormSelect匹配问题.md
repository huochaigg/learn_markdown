
如果搜索用户name‘小苹果’，服务端接口返回的数据是 ‘人名’，
就会过滤掉，怎样让显示出来

```
import { ProFormSelect } from '@ant-design/pro-components';

const MyComponent = () => {
  const fetchData = async (value) => {
    // 这里是调用服务端接口获取数据
    const res = await fetch(`your_api_endpoint?search=${value}`);
    const data = await res.json();
    return data.map(item => ({
      label: item.name,
      value: item.id,
    }));
  };

  return (
    <ProFormSelect
      name="select"
      label="选择"
      request={async (value) => {
        const data = await fetchData(value);
        return data;
      }}
      fieldProps={{
        filterOption: (inputValue, option) => {
          return option?.label?.toLowerCase().includes(inputValue.toLowerCase());
        },
      }}
    />
  );
};

```