# 收信规则

管理自动处理收到邮件的规则。规则写操作需使用真实 `label_id` / `rule_id`，不要猜测 ID。用户在同一请求中要求创建、验证、删除规则时，视为本流程已授权，可使用 `--yes` 通过 CLI 确认门。

## 主题包含文本 → 添加标签

```bash
# 1. 查询真实 label_id
lark-cli mail user_mailbox.labels list --as user \
  --params '{"user_mailbox_id":"me","page_size":20}' --page-all

# 2. 创建规则：主题包含指定文本时添加标签
lark-cli mail user_mailbox.rules create --as user --yes \
  --params '{"user_mailbox_id":"me"}' \
  --data '{"name":"<rule_name>","is_enable":true,"ignore_the_rest_of_rules":false,"condition":{"match_type":1,"items":[{"type":2,"operator":2,"input":"<subject_text>"}]},"action":{"items":[{"type":2,"input":"<label_id>"}]}}'

# 3. 验证规则
lark-cli mail user_mailbox.rules list --as user \
  --params '{"user_mailbox_id":"me"}'

# 4. 删除规则
lark-cli mail user_mailbox.rules delete --as user --yes \
  --params '{"user_mailbox_id":"me","rule_id":"<rule_id>"}'
```

Quick codes above: condition `type=2` = subject, `operator=2` = contains, action `type=2` = add label.

## 原生 API

收信规则走 `user_mailbox.rules` 资源。参数不确定时先运行：

```bash
lark-cli mail user_mailbox.rules -h
lark-cli schema mail.user_mailbox.rules.<method>
```
