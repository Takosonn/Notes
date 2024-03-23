```tcl
set_property AUTO_INCREMENTAL_CHECKPOINT 1 [get_runs impl_1]
#自动增量编译启用

remove_files [get_files -filter {IS_AVAILABLE == 0}]
#工程界面批量删除失效源文件tcl指令

report_utilization
#报告资源使用情况
```

