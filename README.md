from OCC.Core.BRepPrimAPI import BRepPrimAPI_MakeBox, BRepPrimAPI_MakeCylinder
from OCC.Core.gp import gp_Pnt, gp_Vec, gp_Trsf
from OCC.Core.BRepAlgoAPI import BRepAlgoAPI_Fuse
from OCC.Display.SimpleGui import init_display

# 表示の初期化
display, start_display, _, _ = init_display()

# メインの銃本体（長方形のボックス）
main_body = BRepPrimAPI_MakeBox(60, 30, 20).Shape() # 長さ60, 高さ30, 厚み20

# 丸いエネルギータンク（円柱形を追加）
tank1 = BRepPrimAPI_MakeCylinder(10, 30).Shape() # 半径10, 高さ30
tank2 = BRepPrimAPI_MakeCylinder(10, 30).Shape() # もう1つのタンク

# 位置を調整（銃の上に配置）
trsf1 = gp_Trsf()
trsf1.SetTranslation(gp_Vec(15, -10, 20))
trsf2 = gp_Trsf()
trsf2.SetTranslation(gp_Vec(35, -10, 20))

from OCC.Core.BRepBuilderAPI import BRepBuilderAPI_Transform
tank1_trans = BRepBuilderAPI_Transform(tank1, trsf1).Shape()
tank2_trans = BRepBuilderAPI_Transform(tank2, trsf2).Shape()

# 銃本体とエネルギータンクを合成
gun_base = BRepAlgoAPI_Fuse(main_body, tank1_trans).Shape()
gun_base = BRepAlgoAPI_Fuse(gun_base, tank2_trans).Shape()

# 3Dモデルを表示
display.DisplayShape(gun_base, update=True)
start_display()