# Clash Parsers

大多数机场提供的 Clash 订阅链接都包含了 Rules 规则，但是对于用户自定义的规则，Clash提供了Parsers来实现，使用Parsers可以替代Chrome或者Edge的代理软件。

```yaml
parsers:
  - url: # Your subscription address
- 网址： # 您的订阅地址
    code: |
      module.exports.parse = async (raw, { axios, yaml, notify, console }, { name, url, interval, selected }) => {
module.exports.parse = async （raw， { axios， yaml， notify， console }， { name， url， interval， selected }） => {
        const obj = yaml.parse(raw);
        const address = "https://raw.githubusercontent.com/guiqiqi/storage/master/rules.yaml";
常量地址 =“https://raw.githubusercontent.com/guiqiqi/storage/master/rules.yaml”;
        
        // Configure for DNS settings
针对 DNS 设置进行配置
        obj.dns['default_nameserver'] = [
          '8.8.8.8', 
          '1.1.1.1'
        ];
        obj.dns.nameserver = [
          'tls://8.8.4.4:853',
          'https://dns.cloudflare.com/dns-query'
        ];
        obj.dns.fallback = [
          'https://doh.pub/dns-query',
          'https://dns.alidns.com/dns-query',
          'https://doh.dns.sb/dns-query',
          'https://dns.twnic.tw/dns-query'
        ];
        obj.dns['enhanced-mode'] = 'fake-ip';

        // Configure proxy groups and rules
配置代理组和规则
        const proxies = [];
        for (const proxy of obj.proxies) {
for （obj.proxies 的 const proxy ） {
          proxies.push(proxy.name);
        }
        obj['proxy-groups'] = [
          {
            name: 'Proxy',
            type: 'url-test',
            proxies: proxies,
            url: 'http://www.gstatic.com/generate_204',
            interval: interval
间隔：间隔
          }
        ];
        const rules = await axios.get(address).then(function (response) {
const rules = await axios.get（address）.then（function （response） {
          return yaml.parse(response.data).rules;
        });
        obj.rules = rules;

        // Popup notifications
        const title = "更新成功";
        const message = "更新并预处理了订阅文件: " + name;
        notify(title, message);
通知（标题、消息）;
        return yaml.stringify(obj);
      }
```