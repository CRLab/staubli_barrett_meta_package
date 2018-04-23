Barrett Model
=============

This repository contains a description of a BarrettHand 280 mounted on a Staubli TX60L arm. It uses the BarretHand description barrett_model repo that can be found at https://github.com/CURG/barrett_model and the staubli repo found at https://github.com/CURG/staubli.

Currently, the Staubli model is actually the RX60L, but they seem to be more or less identical kinematically.

The offset from the BarrettHand to the Staubli can be configured by creating a new xacro file containing the hand_xyz_offset and hand_rpy_offset properties as described in the demonstration config/arm_to_hand_calib.xacro

The main xacro file can be found in robots/staubli_bhand.xacro

The mount apparatus (which currently consists of an ATI load cell and the mounting device provided by Barrett Technologies) is described in urdf/barrett_hand_mount.urdf.xacro using simple geometry. 