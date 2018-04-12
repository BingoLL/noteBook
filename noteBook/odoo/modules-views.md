## odoo模块开发

建议按照官方示例排放各段代码。菜单栏一定要放在视图的下面，否则会报错

```
    <!-- explicit list view definition 创建模型数据视图 -->

    <record model="ir.ui.view" id="charttest.list">
      <field name="name">charttest list</field>
      <field name="model">charttest.charttest</field>
      <field name="arch" type="xml">
        <tree>
          <field name="name"/>
          <field name="value"/>
          <field name="value2"/>
        </tree>
      </field>
    </record>
```
```

    <!-- actions opening views on models 所被触发的视图-->

    <record model="ir.actions.act_window" id="charttest.action_window">
      <field name="name">charttest window</field>
      <field name="res_model">charttest.charttest</field>
      <field name="view_mode">tree,form</field>
    </record>
```
```
    <!-- server action to the one above -->

    <record model="ir.actions.server" id="charttest.action_server">
      <field name="name">charttest server</field>
      <field name="model_id" ref="model_charttest_charttest"/>
      <field name="state">code</field>
      <field name="code">
        action = {
          "type": "ir.actions.act_window",
          "view_mode": "tree,form",
          "res_model": self._name,
        }
      </field>
    </record>
```

```
    <!-- Top menu item 顶部菜单名 -->

    <menuitem name="charttest" id="charttest.menu_root"/>
```
```
    <!-- menu categories 侧边分类目录 -->

    <menuitem name="Menu 1" id="charttest.menu_1" parent="charttest.menu_root"/>
    <menuitem name="Menu 2" id="charttest.menu_2" parent="charttest.menu_root"/>
```
```
    <!-- actions 侧边分类子目录 并触发相应的视图 -->

    <menuitem name="List" id="charttest.menu_1_list" parent="charttest.menu_1"
              action="charttest.action_window"/>
    <menuitem name="Server to list" id="charttest" parent="charttest.menu_2"
              action="charttest.action_server"/>
```