cmake_minimum_required(VERSION 3.16)
project(TrenchCoatShop)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_AUTOMOC ON)


find_package(Qt6 REQUIRED COMPONENTS Widgets)

add_executable(TrenchCoatShop
    main.cpp
    # Qt UI layer
    MainWindow.h        MainWindow.cpp
    AdminWidget.h       AdminWidget.cpp
    UserWidget.h        UserWidget.cpp
    BasketDialog.h      BasketDialog.cpp
    BasketModel.h       BasketModel.cpp

    # domain layer
    TrenchCoat.h        TrenchCoat.cpp
    IRepository.h
    FileRepository.h    FileRepository.cpp
    Service.h           Service.cpp
    CoatValidator.h     CoatValidator.cpp
    Exceptions.h
    FileBasket.h
    CSV_Basket.h        CSV_Basket.cpp
    HTML_Basket.h       HTML_Basket.cpp

)

target_link_libraries(TrenchCoatShop PRIVATE Qt6::Widgets)
