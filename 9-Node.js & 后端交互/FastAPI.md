以下文件
- item_router:接口索引到处理逻辑
- crud：数据库操作
- schemas：定义数据表
	![[Pasted image 20250325153533.png]]


解读文章item点赞的后端实现：
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