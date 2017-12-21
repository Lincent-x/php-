注意在socket_bind的时候ip地址不能真回环地址如127.0.0.1
server.php后台跑起来的时候nohup php server.php > /var/tmp/a.log 2>&1 &
#udp 方式

##server.php
`       
   
        <?php
        //error_reporting( E_ALL );
 
         set_time_limit( 0 );
 
        ob_implicit_flush();
 
         $socket = socket_create( AF_INET, SOCK_DGRAM, SOL_UDP );
 
         if ( $socket === false ) {
 
         echo "socket_create() failed:reason:" . socket_strerror( socket_last_error() ) . "\n";
 
        }
 
         $ok = socket_bind( $socket, '202.85.218.133', 11109 );
 
        if ( $ok === false ) {
 
             echo "socket_bind() failed:reason:" . socket_strerror( socket_last_error( $socket ) );
 
         }
 
         while ( true ) {
  
            $from = "";
  
            $port = 0;
  
            socket_recvfrom( $socket, $buf,1024, 0, $from, $port );
  
            echo $buf;
  
             usleep( 1000 );
   
             }
 
        ?>`

##client.php

`


    <?php

    $sock = socket_create(AF_INET, SOCK_DGRAM, SOL_UDP);
 
    $msg = 'hello';
 
    $len = strlen($msg);
 
    socket_sendto($sock, $msg, $len, 0, '202.85.218.133', 11109);
 
    socket_close($sock);
 
    ?>`

# TCP 方式

##server.php
`

     <?php

     //error_reporting( E_ALL );

    set_time_limit( 0 );
 
    ob_implicit_flush();
 
    $socket = socket_create( AF_INET, SOCK_STREAM, SOL_TCP );
 
    socket_bind( $socket, '192.168.2.143', 11109 );
 
    socket_listen($socket);
 
    $acpt=socket_accept($socket);
 
    echo "Acpt!\n";
 
    while ( $acpt )  {
 
    $words=fgets(STDIN);
   
    socket_write($acpt,$words);
   
    $hear=socket_read($acpt,1024);
   
    echo $hear;
   
    if("bye\r\n"==$hear){
   
    socket_shutdown($acpt);
     
    break;
     
    }
   
    usleep( 1000 );
   
    }
 
    socket_close($socket)
 
    ?>`

## client.php
`

     <?php
    
    $socket = socket_create(AF_INET, SOCK_STREAM, SOL_TCP);
    
    $con=socket_connect($socket,'192.168.2.143',11109);
    
    if(!$con){socket_close($socket);exit;}
    
    echo "Link\n";
    
    while($con){
    
    $hear=socket_read($socket,1024);
    
    echo $hear;
    
    $words=fgets(STDIN);
    
    socket_write($socket,$words);
    
    if($words=="bye\r\n"){
        break;
    }
    }
    socket_shutdown($socket);
    socket_close($sock);
    ?>`
