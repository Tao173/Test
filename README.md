## 测试用虚拟按钮，可删除
input_boolean:
  test11111:
    name: 虚拟开关
  test22222:
    name: 虚拟自动化

## 计数器
counter:
  my_custom_counter:
    initial: 0
    step: 1
    restore: false

automation:
  ## 5秒内切换2次开关状态，自动化开关状态切换
  ## 每次切换开关状态，计数器+1
  - alias: test1
    hide_entity: true
    trigger:
      platform: state
      entity_id: input_boolean.test11111   #替换为自己的开关
    action:
      service: counter.increment
      entity_id: counter.my_custom_counter
  ## 计数器为2的时候，切换自动化状态
  - alias: test2
    hide_entity: true
    trigger:
      - platform: state
        entity_id: counter.my_custom_counter
        to: '2'
    action:
      service: input_boolean.toggle        #替换为自己的自动化
      entity_id: input_boolean.test22222   #替换为自己的自动化
  ## 每次计数器状态变动，5秒后重置归零
  - alias: test3
    hide_entity: true
    trigger:
      - platform: state
        entity_id: counter.my_custom_counter
    action:
      - delay: 00:00:05
      - service: counter.reset
        entity_id: counter.my_custom_counter
