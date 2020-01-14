```FLASK
### FLASK数据库的增删改查---操作笔记
# set FLASK_APP=app.py
# flask shell
from app import Role, User

###  创建数据
admin_role = Role(name='Admin')
mod_role = Role(name="Moderator")
user_role=Role(name='User')
user_john=User(username='john',role=admin_role)
user_susan = User(username='susan', role=admin_role)
user_david=User(username='david',role=admin_role)

### 存入数据集
from app import db
db.session.add(admin_role)
db.session.commit()
db.session.add(mod_role)
db.session.add(user_role)
db.session.commit()
print(user_role)

###id存在，重新修改name
admin_role.name="Administrator"
db.session.add(admin_role)
db.session.commit()

###删除
db.session.delete(mod_role)
db.session.commit()

###查询
User.query.all()
Role.query.all()
User.query.filter_by(role=user_role).all()
###查看生成的SQL
str(User.query.filter_by(role=user_role))
```
