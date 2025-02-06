1. compile boost  
./bootstrap.sh --prefix=/opt/boost/1.85.0  
./b2  
./b2 headers  
sudo ./b2 install   

1. compile msgpack-c  
cmake -DCMAKE_PREFIX_PATH=/opt/boost/1.85.0/include -DCMAKE_PREFIX_PATH=/opt/boost/1.85.0/lib -DCMAKE_INSTALL_PREFIX=/opt/msgpack/msgpack-c .    
sudo cmake --build . --target install  

