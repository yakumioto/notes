- Unstarted task list
  template:: Daily
	- {{query (and (task todo later) (between -1y yesterday))}}
	-
- [[Weekly]]
  template:: Weekly
	- 本周完成情况
	- 本周输出成果
	- 下周工作计划
	- {{query (and (task doing done) (between -1w yesterday))}}
	-