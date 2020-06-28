# CCNP EIGRP 2
Enhanced Interior Gateway Routing Protocol

# 實際操作


                                   ISP Router 1
                     

                                     RouterA
                                     
                                     
                                     
                         RouterB                 RouterC


(1) 將各介面的 IP 位址設定好。

                                 AS200: 172.16.11.0
                                        |
                                    ISP Router 1
                               


                                10.2.2.12/255.255.255.0
                                        ｜
                                     RouterA
                                     /  |       \
              192.168.2.49/255.255.255.240       192.168.2.33/255.255.255.240
                                        |                \
                              192.168.2.17/255.255.255.240\
                             /                    \        \
                            /                      \         \
                       192.168.2.50          192.168.2.18   192.168.2.34/255.255.255.240
                          /                  |        \        \
         RouterC - 192.168.2.16/255.255.255.240        RouterB   
                                                           |
                                                           192.168.2.65/255.255.255.240
                                                           
 (2) 利用指令 router eigrp 在路由上使用 EIGRP 設定。
 
 
         RouterC# conf t
         RouterC(config)#router eigrp 200
         RouterC(config-router)#network 192.168.2.0
         
         ctrl + Z
         
         
  (3) 在 ISP 路由器上設定 EIGRP 網段，包含 10.0.0.0 和 172.16.0.0，設定完成後，使用指令 sh run 來做檢視。
  
  
        （略）
        
         ISP_Router1#sh run
         !
         hostname ISP_Router1
         !
         ip subnet-zero
         !
         no ip domain lookup
         !
         --More--
         int lo0
         ip address 172.16.1.100 255.255.255.0
         no ip directed-broadcast
         (略)
         ！
         int s0
         ip address 10.1.1.100 255.255.255.20
         no ip directed-broadcast
         ip summary-address eigrp 172.16.0.0 255.255.0.0 5
         ip summary-address eigrp 10.0.0.0 255.0.0.0 5
         no ip mroute-cache
         no fair-queue
         (略)
         !
         router eigrp 200
         network 10.0.0.0
         network 172.16.0.0
         !
         ip classless
         no ip http server
         !
         line con 0
         exec-timeout 0 0
         logging sync
         transport input none
         line aux 0
         line vty 0 4
             exec-timeout 0 0
             logging sync
             no login
         
         
        
        
    
 
                                                           
