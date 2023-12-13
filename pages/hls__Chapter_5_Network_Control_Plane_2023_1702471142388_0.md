file:: [Chapter_5_Network_Control_Plane_2023_1702471142388_0.pdf](../assets/Chapter_5_Network_Control_Plane_2023_1702471142388_0.pdf)
file-path:: ../assets/Chapter_5_Network_Control_Plane_2023_1702471142388_0.pdf

- 网络层
  ls-type:: annotation
  hl-page:: 5
  hl-color:: yellow
  id:: 6579a658-89a5-4bb3-8fb4-9f6821874131
- all routers have complete topology, 
  ls-type:: annotation
  hl-page:: 11
  hl-color:: yellow
  id:: 6579a76a-bfa1-42e0-8f1b-e066192751c0
- 静态路由算法
  ls-type:: annotation
  hl-page:: 26
  hl-color:: yellow
  id:: 6579b9c8-ff82-4e46-8d7a-e9777c586976
- Bellman-Ford (BF)
  ls-type:: annotation
  hl-page:: 29
  hl-color:: yellow
  id:: 6579bbe1-fdf8-478b-961e-5db24d2cc084
- Da(a)=0
  ls-type:: annotation
  hl-page:: 43
  hl-color:: yellow
  id:: 6579c300-1713-4c5c-9cde-2cd7385ddd9e
- Dc(a) = ∞
  ls-type:: annotation
  hl-page:: 43
  hl-color:: yellow
  id:: 6579c313-492d-45f9-b9cc-897865d4b023
- De(a) = ∞
  ls-type:: annotation
  hl-page:: 43
  hl-color:: yellow
  id:: 6579c316-8d39-467a-acaa-ba9064046036
- Db(a) = min{cb,a+Da(a), cb,c +Dc(a), cb,e+De(a)} = min{8,∞,∞} = 8
  ls-type:: annotation
  hl-page:: 43
  hl-color:: yellow
  id:: 6579c31c-826a-4250-82f5-78bdb3618368
- Da(c) = ∞
  ls-type:: annotation
  hl-page:: 43
  hl-color:: yellow
  id:: 6579c325-b280-4a8d-89ec-a204cd92f49c
- Dc(c) = 0
  ls-type:: annotation
  hl-page:: 43
  hl-color:: yellow
  id:: 6579c32c-3843-4ee4-8d01-abcf13a984f4
- De(c) = ∞
  ls-type:: annotation
  hl-page:: 43
  hl-color:: yellow
  id:: 6579c331-2841-48f0-a990-a61a21d356d2
- Db(c) = min{cb,a+Da(c), cb,c +Dc(c), c b,e +De(c)} = min{∞,1,∞} = 1
  ls-type:: annotation
  hl-page:: 43
  hl-color:: yellow
  id:: 6579c341-308b-4f82-a106-704d27d1b863