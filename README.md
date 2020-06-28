# CCNP EIGRP 2
Enhanced Interior Gateway Routing Protocol

# 實際操作


                                   ISP Router 1
                     

                                     RouterA
                                     
                                     
                                     
                         RouterC                 RouterB
  


(1) 將各介面的 IP 位址設定好。

                                 AS200: 172.16.11.0
                                        |
                                    ISP Router 1
                               
                                        s1
                                10.2.2.100/255.255.255.0
                                
                                        |

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
                                                           
 (2) 利用指令 router eigrp 在路由器 C 上使用 EIGRP 設定。
 
 
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
         ip address 10.1.1.100 255.255.255.0
         no ip directed-broadcast
         ip summary-address eigrp 172.16.0.0 255.255.0.0 5
         ip summary-address eigrp 10.0.0.0 255.0.0.0 5
         no ip mroute-cache
         no fair-queue
         clockrate 56000
         !
         int s1
         ip address 10.2.2.100 255.255.255.0
         no ip directed-broadcast
         clockrate 56000
         !
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
         !
         end
         
         
  (4) 為路由器 A 設定 EIGRP 網段，包含 10.0.0.0 和 192.168.2.0，設定完成後，使用指令 sh run 檢視路由器資訊。
  
  (5) 為路由器 B 設定 EIGRP 網段 192.168.2.0，設定完成後，使用指令 sh run 檢視路由器資訊。
        
  (6) 在 ISP 路由器上，執行命令 show ip route，查看路由表結果。
      結果顯示路由器透過 10.2.2.2 此目標 IP 位址，學習到 192.168.2.0 網段。
  
  
          ISP_Router1#sh ip route
          
          // mode 中 EIGRP 代表 D, connected 代表 C
          
          C
          C
          C (略)
          
          D 192.168.1.0/255.255.255.0 via 10.1.1.1, S0
            192.168.2.0/255.255.255.0 is variably subnetted, 2 subnets, 2 masks
           // int s0
           // ip address 10.1.1.100 255.255.255.0
           
          D 192.168.2.0/255.255.255.128 via 10.2.2.2, S1
            192.168.2.0/255.255.255.0 via 10.2.2.2, S1
           // int s1
           // ip address 10.2.2.100 255.255.255.0
          
  (6) 如同步驟 6，使用同樣指令 sh ip route 檢視 RouterA、B、C 的路由表。
    
  # 路徑自動集合
   
    auto summary 
 
  to be continued...
  
  # 拓樸表的檢視
  
       RouterA#sh ip eigrp topology
             
             codecs: P - passive, A - active, U - update, Q -q uery, R - reply, r - reply status
                     //FD feasible distance
             
             P 10.0.0.0/8, 1 succesors, FD
             P 10.2.2.0/24, 1 succesors, FD, via connected, serial 1/3
             
             (略)
  
  
