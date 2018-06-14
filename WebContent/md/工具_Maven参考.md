# 依赖范围与classpath关系

| 依赖范围（Scope） | 编译 | 测试 | 运行时 | 例子         |
| ----------------- | ---- | ---- | ------ | ------------ |
| compile           | Y    | Y    | Y      |              |
| test              | -    | Y    | -      |              |
| provided          | Y    | Y    | -      | servlet-api  |
| runtime           | -    | Y    | Y      | JDBC驱动实现 |
| system            | Y    | Y    | -      | 本地类库文件 |

# 传递性依赖和依赖范围

|          | compile  | test | provided | runtime  |
| -------- | -------- | ---- | -------- | -------- |
| compile  | compile  | -    | -        | runtime  |
| test     | test     | -    | -        | test     |
| provided | provided | -    | provided | provided |
| runtime  | runtime  | -    | -        | runtime  |

# 远程仓库配置-updatePolicy元素

| 候选值          | 说明             |
| --------------- | ---------------- |
| daily (default) | 每天检查一次更新 |
| never           | 从不更新         |
| always          | 每次构建更新     |
| interval        | 每隔X分钟更新    |

# 远程仓库配置-checksumPolicy元素

| 候选值         | 说明               |
| -------------- | ------------------ |
| warn (default) | 构建时输出警告信息 |
| fail           | 构建失败           |
| ignore         | 忽略校验和错误     |

# 生命周期

| 生命周期 | 阶段                    | 内置插件绑定                         | 说明                   |
| -------- | ----------------------- | ------------------------------------ | ---------------------- |
| clean    |                         |                                      | 清理项目               |
|          | pre-clean               |                                      |                        |
|          | clean                   | maven-clean-plugin:clean             | 清理上次构建生成的文件 |
|          | post-clean              |                                      |                        |
| default  |                         |                                      | 构建项目               |
|          | validate                |                                      |                        |
|          | initialize              |                                      |                        |
|          | generate-sources        |                                      |                        |
|          | process-sources         |                                      |                        |
|          | generate-resources      |                                      |                        |
|          | process-resources       | maven-resources-plugin:resources     |                        |
|          | compile                 | maven-compiler-plugin:compile        |                        |
|          | process-classes         |                                      |                        |
|          | generate-test-sources   |                                      |                        |
|          | process-test-sources    |                                      |                        |
|          | generate-test-resources | maven-resources-plugin:testResources |                        |
|          | process-test-resources  |                                      |                        |
|          | test-compile            | maven-compiler-plugin:testCompile    |                        |
|          | process-test-classes    |                                      |                        |
|          | test                    | maven-surefire-plugin:test           |                        |
|          | prepare-package         |                                      |                        |
|          | package                 | maven-jar-plugin:jar                 |                        |
|          | pre-integration-test    |                                      |                        |
|          | integration-test        |                                      |                        |
|          | post-integration-test   |                                      |                        |
|          | verify                  |                                      |                        |
|          | install                 | maven-install-plugin:install         |                        |
|          | deploy                  | maven-deploy-plugin:deploy           |                        |
| site     |                         |                                      | 建立项目站点           |
|          | pre-site                |                                      |                        |
|          | site                    | maven-site-plugin:site               |                        |
|          | post-site               |                                      |                        |
|          | site-deploy             | maven-site-plugin:deploy             |                        |

# 反应堆裁剪选项

| 选项 | 全称                  | 说明                           |
| ---- | --------------------- | ------------------------------ |
| -am  | --also-make           | 同时构建所列模块的依赖模块     |
| -amd | -also-make-dependents | 同时构建依赖于所列模块的模块   |
| -pl  | --projets \<arg\>     | 构建指定模块，模块间用逗号分隔 |
| -rf  | -resume-from \<arg\>  | 从指定模块回复反应堆。         |

