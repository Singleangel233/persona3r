persona3Reload人格面具配置拆分：

1、人格面具基础属性总表personaDB
| INT_id | STRING_name | STRING_appearance | INT[]_strength | INT[]_intelligence | INT[]_defense | INT[]_speed | INT[]_luck | INT[]_baseHP | INT[]_baseMP | INT_baseLevel |INT_personaType | INT[]_baseSkill | INT[][]_upgradeSkill | INT_resistanceID | INT[]_itemGet |
|--------|-------------|-------------------|----------------|--------------------|---------------|-------------|------------|--------------|--------------|-----------------|----------------|-----------------|----------------------|------------------|------------|
| 用于表示此配置id项 | 用于表示人格面具的名称 | 用于表示人格面具的立绘形象资源路径 | [升级增加的权重比,初始力量值] |[升级增加的权重比,初始魔力值]|[升级增加的权重比,初始耐力值]|[升级增加的权重比,初始速度值]|[升级增加的权重比,初始幸运值]|[升级增加的点数,初始HP值]|[升级增加的点数,初始MP值]|[对应最低初始等级]|对应人格面具的类型（0-22）|[初始技能id]（具体需要查看skill技能表）|[[等级需求，技能id],...]|抗性id（索引到personaResistance表）|[升级次数,获取的道具id（索引item道具表）]

\
 2、抗性表
对于id是根据到人格面具personaDB的INT_resistanceID来进行索引配置的 

属性抗性分为四类，1：普通受击，2：减半，3：抵抗：4：反射，5：吸收

例如下图所示的路西法（假设id位180）表示的抗性，那么可以配置成
![Alt text](https://github.com/Singleangel233/persona3r/blob/main/picfile/1.jpg?raw=true "Optional title")

| INT_id | INT_physicsSlash | INT_physicsPunch | INT_phyciscShoot | INT_fire | INT_ice | INT_bolt | INT_wind | INT_dark | INT_light|
|--------|-------------------|-----------------|------------------|----------|---------|----------|----------|----------|----------|
|180     | 1|1|1|5|1|5|1|1|4|

\
3、技能配置表skillDB

技能类型分为，

1：伤害类，使用造成伤害，注意：1-1（subType子类）物理伤害类型；1-2 魔法伤害类型；1-3特殊类（例如光即死和暗即死）；1-4 多次攻击类（例如多次的物理攻击）

2：buff类，使用后生成buff或者debuff，注意：2-1 增益buff类；2-2 对敌debuff类；2-3 蓄力类；2-4 清除我方/敌方buff类（迪卡加这类）

3：治疗类，使用可以治愈hp或者异常状态（包括复活），注意：3-1 hp回复类；3-2 异常状态回复类；

4：被动类，不能使用，但是会有额外加成，注意：4-1 伤害增益类；4-2 减半抗性类；4-3 无效抗性类；4-4 反射抗性类； 4-5 吸收抗性类；4-6 状态免疫类； 

5：被动回复类，开辟一个新的分支，方便用于程序判断，注意：5-1 hp每回合恢复类，5-2 mp每回合回复类； 5-3 hp，mp战斗后回复类；5-4 战斗开始释放增益类；

6：反射类，特殊buff类技能，但是效果是反射 物理/属性魔法 技能 

| INT_id | STRING_name | INT_Type |INT_subType| INT_basePower | INT_status | INT_element | INT_attribute|INT_costPower | INT_effectObject | INT_turn |INT[]_extra| STRING_performance | STRING_icon |
|---------------|----------|-----------|---------------|------------|-------------|----------|-----------|--------------------|---------|-------|-------|----|----|
|用于表示技能id项|用于表示技能名称|用于表示技能主要类型|用于表示技能次要类型，主要作为标记|基础技能数值|给予异常状态|给予的对象属性增减，根据能力分为次要的5种|作用对象，分为5种（己方单体，己方全体，敌方单体，敌方全体，敌我全体）|持续回合（2,6类型适用）|额外字段，这个会根据主类子类结合提供数值参考，用于处理一些特殊逻辑，例如buff类到底是增加还是减少，或者清除buff到底是哪些buff作用|技能表现展现特效资源路径|技能icon
