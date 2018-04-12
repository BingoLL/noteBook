## 模型函数 model

```
# -*- coding: utf-8 -*-

from odoo import models, fields, api

 class charttest(models.Model):
     _name = 'charttest.charttest'
     name = fields.Char(string='可以为这个字段起一个名字')
     value = fields.Integer(string='每个字段所起的名字将会在页面视图中显示')
     value2 = fields.Float(compute="_value_pc", store=True)
     description = fields.Text()

     @api.depends('value')
     def _value_pc(self):
         self.value2 = float(self.value) / 100

```