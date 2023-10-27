# Clash Parsers

大多数机场提供的 Clash 订阅链接都包含了 Rules 规则，但是对于用户自定义的规则，Clash提供了Parsers来实现，使用Parsers可以替代Chrome或者Edge的代理软件。

```yaml
parsers:
  - url: #your code here
    code: |
      module.exports.parse = async (raw, { axios, yaml, notify, console }, { name, url, interval, selected }) => {
        const obj = yaml.parse(raw);
        const address = "https://raw.githubusercontent.com/mtraders/clash-parsers/main/rules.yaml";
        
        const rules = await axios.get(address).then(function (response) {
          return yaml.parse(response.data).rules;
        });
        obj.rules = rules;

        // Popup notifications
        const title = "更新成功";
        const message = "更新并预处理了订阅文件: " + name;
        notify(title, message);
        return yaml.stringify(obj);
      }
```