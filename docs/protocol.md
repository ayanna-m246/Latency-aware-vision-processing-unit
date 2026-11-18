# Board <-> laptop protocol (WebSocket, port 8765)

| direction | message | content |
|---|---|---|
| board -> laptop | binary v2 | `b"VPU2"` + `[uint32 frame_id][float64 capture_time_s][uint32 source_generation]` + JPEG |
| board -> laptop | binary v1 | `[uint32 frame_id]` + JPEG (old boards still work) |
| board -> laptop | JSON | `{"cal": {"hfov","cam_h","horizon","foe_x"}}` whenever calibration changes |
| board -> laptop | JSON | `{"ping": n}` |
| laptop -> board | JSON | `{"id","dets":[[cls,conf,x1,y1,x2,y2]...],"ext":[{loom,u,v,n}|null...],"ego":{ok,v,yaw,pitch,horizon,foe_x,...},"yolo_ms","motion_ms","infer_ms"}` (`ext`/`ego` only for v2) |
| laptop -> board | JSON | `{"pong": n, "v": 2}` |

Boxes are normalised (0-1). `--token SECRET` on the server requires clients to connect with `?token=SECRET`.
Full details: docstring at the top of [`laptop/server.py`](../laptop/server.py).
