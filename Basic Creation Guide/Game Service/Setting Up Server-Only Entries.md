# Course Introduction
When a creator creates a world, there may be a function in DataSet or a script that the creator does not want to disclose to users who play the game due to security issues. Today, let's learn how to set DataSet or a function to server-only.

# The Need for Server-Only Entries
When creating a world, for example, it is safest to ensure that information such as coupon codes, important keys used in the game, damage formulas, or internal logic is processed only on the server. If such sensitive information exists on both the server and the client, it may be a security vulnerability because the client can also access the sensitive information. In this case, you need to solve the security issue by setting it so that the important DataSet or the implementation part of the functions in the script are not transferred to the client.

# Server-Only DataSet settings
If you check the ServerOnly checkbox in the DataSet's Property ![property](https://mod-file.dn.nexoncdn.co.kr/storage/icons/tab/icon_inspector.png "property"), you can process it so that the DataSet content is not delivered to the client when the game world is played.
![1](https://mod-file.dn.nexoncdn.co.kr/bbs/1661169318239863cf967cd4a46d3b1467ddcf4659ddc.png "1")
DataSet set to ServerOnly cannot be accessed by clients, so creators have to write scripts with this in mind. If some data from that DataSet is needed at a specific timing, it needs to be passed to the client, such as by execution control or synchronization.

# Setting the Execution Space of a Function
If the execution space of a function in the script is set to ServerOnly or Server, the implementation of that function is not passed to the client.
1. After clicking the function setting button, select the Execution Space Setting menu.
    ![3](https://mod-file.dn.nexoncdn.co.kr/bbs/1661216740609c9bbe61a337e4b62ba90e21ab21e65e2.png "3")
2. Set the execution space to Server or ServerOnly.
    ![4](https://mod-file.dn.nexoncdn.co.kr/bbs/1727152502360be98b8648a034dcfb8a09a5a5855ba30.png "4")
    The implementation of the function set in this way is not passed to the client.
    ![2](https://mod-file.dn.nexoncdn.co.kr/bbs/166117015841552bb10e2fffd476b82770fe72c0a5787.png "2")