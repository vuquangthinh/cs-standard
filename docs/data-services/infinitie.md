
Đọc trước phần này để hiểu rõ về tính chủ động, bị động. [Active-Passive](core-concepts/active-passive.md)

Có nhiều phương án để thực hiện:
- Convert từ page / pageSize khi scroll tới ngưỡng (threshold) thì tăng page lên 1
- Sử dụng điểm neo. Khi tới ngưỡng thì lấy các record sau id. Sau khi tới ngưỡng,  cập nhật danh sách và cập nhật id.

Phân tích: 
- Người dùng scroll để tải thêm dữ liệu là hành động chủ động.

- Khi người dùng scroll tới ngưỡng threshhold -> 1 hành động chủ động sinh ra -> gọi tạm là event onLoadMore -> thực hiện append thêm dữ liệu vào mảng có sẵn.

- Khi người dùng pull to refresh -> 1 hành động chủ động sinh ra -> onRefresh event -> thực hiện việc làm mới mảng hiện tại.

- Khi người dùng mở danh sách lên lần đầu tiên -> Hiển thị một số lượng item nhất định
trong lần đầu tiên -> hành động thực hiện bị động.


Sample implementation với Dropdown Select ReactJS

```
import React, { useState, useCallback, useEffect, useRef } from 'react';
import { Select, Spin } from 'antd';

const { Option } = Select;

const ServiceSelectImpl = ({
  service,
  params,
  renderOptionTitle,
  initialList,
  initialLimit = 10,
  initialOffset = 0,
  ...rest
}) => {
  const [list, setList] = useState();
  const [loadingMore, setLoadingMore] = useState(false);
  const [loading, setLoading] = useState(false);
  const limit = useRef(initialLimit);
  const offset = useRef(initialOffset);
  const noMore = useRef(false);

  const loadMore = useCallback(async () => {
    try {
      setLoadingMore(true);
      offset.current += limit.current;

      const result = await service({
        ...params,
        limit: limit.current,
        offset: offset.current,
      });

      if (result.success) {
        setList([...list, ...result.data.list]);
      } else {
        // ERROR
      }
      setLoadingMore(false);
    } catch (error) {}
  }, [list, params, service]);

  const loadData = useCallback(
    async ({}) => {
      try {
        setLoading(true);
        const result = await service({
          ...params,
          limit: limit.current,
          offset: offset.current,
        });

        if (result.success) {
          setList(result.data.list);
          if (result.data.list.length < limit.current || result.data.total <= offset.current) {
            noMore.current = true;
          } else {
            noMore.current = false;
          }
        } else {
          // ERROR
        }
        setLoading(false);
      } catch (error) {}
    },
    [params, service],
  );

  const onPopupScroll = e => {
    e.persist();
    let target = e.target;
    if (
      !loading &&
      !loadingMore &&
      !noMore.current &&
      target.scrollTop + target.offsetHeight === target.scrollHeight
    ) {
      // dynamic add options...
      loadMore();
    }
  };

  useEffect(() => {
    let isMounted = true;
    const timer = setTimeout(() => isMounted && loadData({}), 500);
    return () => {
      isMounted = false;
      clearTimeout(timer);
    };
  }, [loadData]);

  return (
    <Select
      {...rest}
      onPopupScroll={onPopupScroll}
      loading={loading}
      onSearch={onSearch}
      onFocus={onFocus}
    >
      {list.map(item => (
          <Option originObj={item.originObj} key={item.id} value={`${item.value}`}>
            {renderOptionTitle ? renderOptionTitle(item) : item.title}
          </Option>
        ))}
      {loadingMore && (
        <Option key={'loading'} value={'loading'} disabled>
          <Spin size={'small'} />
        </Option>
      )}
    </Select>
  );
};
```