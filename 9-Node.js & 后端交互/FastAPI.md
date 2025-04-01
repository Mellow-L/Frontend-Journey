以下文件
- item_router:接口索引到处理逻辑
- crud：数据库操作
- schemas：定义数据表

### 解读文章item点赞的后端实现：
```python
@app.post("/items/put/addstar/{item_id}", response_model=schemas.Item)
def add_item_star(
	item_id: int,
	db: Session = Depends(get_db),
	auth_user: User = Depends(current_active_user)
):
    uuid = str(auth_user.id)
    db_item= crud.get_items_by_id(db, item_id)
    if not db_item:
        raise HTTPException(status_code=404, detail="Item not found")
    return crud.modify_item_star(db, item_id, 1)
```
- 请求方法：`POST`
- 请求路径：`/items/put/addstar/{item_id}`，其中 `{item_id}` 是动态参数，表示要点赞的 `item` 的 ID。
- **response_model 规定返回参数符合 schemas.Item 结构**
	- 在schema.py中：定义了class Item
- 定义item_id
- `db` 是 **数据库会话对象**，用于执行数据库操作。
	- `get_db` 是一个 FastAPI 依赖
- 这里将一个py函数在入参后就换行了。



### 后端点赞用户表的实现
在`sql_app/models.py`文件中添加点赞表模型：
```python
class Like(Base):
    __tablename__ = "likes"
    id = Column(Integer,primary_key = True,index = True)
    item_id = Column(Integer,ForeignKey("items.id",ondelete="CASCADE"))
    user_uuid = Column(String,ForeignKey("user.uuid",ondelete="CASCADE"))
    create_time = Column(DateTime,default=datetime.now)
    item = relationship("Item", back_populates="likes")
    user = relationship("User", back_populates="likes")
```
- `item = relationship("Item", back_populates="likes")`
	- 在Like表中定义item字段，关联Item表。
	- back_populates指定Item表中关联本Like表的字段为likes。
- 在表模型中定义表名称、字段（指定主外键等），添加表关联。

在`sql_app/crud.py`文件中添加对Like表的操作函数：
