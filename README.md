```swift

//
//  ContentView.swift
//  scrollable_tabbar
//
//  Created by paige on 2022/02/03.
//

import SwiftUI

struct ContentView: View {
    
    @State var currentTab: Int = 0
    
    var body: some View {
        ZStack(alignment: .top) {
            TabView(selection: self.$currentTab) {
                View1().tag(0)
                View2().tag(1)
                View3().tag(2)
            }
            .tabViewStyle(.page(indexDisplayMode: .always))
            .edgesIgnoringSafeArea(.all)
            
            TabBarView(currentTab: self.$currentTab)
        }
    }
}

struct TabBarView: View {

    @Binding var currentTab: Int
    @Namespace var namespace
    var tabBarOptions: [String] = ["Item1", "Item2", "Item3"]
    
    var body: some View {
        ScrollView(.horizontal, showsIndicators: false) {
            HStack(spacing: 20) {
                ForEach(Array(zip(self.tabBarOptions.indices, self.tabBarOptions)),
                        id: \.0, content: { index, name in
                    TabBarItem(currentTab: self.$currentTab, namespace: namespace.self, tab: index, tabBarItemName: name)
                })
            }
        }
        .background(.white)
        .frame(height: 80)
        .edgesIgnoringSafeArea(.all)
    }
}


struct TabBarItem: View {
    
    @Binding var currentTab: Int
    let namespace: Namespace.ID
    
    var tab: Int
    var tabBarItemName: String
    
    
    var body: some View {
        Button {
            self.currentTab = tab
        } label: {
            VStack {
                Spacer()
                Text(tabBarItemName)
                    .foregroundColor(.black)
                // comment this line if you dont want bottom bar 
                if currentTab == tab {
                    Color.black.frame(height: 2)
                        .frame(height: 2)
                        .matchedGeometryEffect(id: "underline", in: namespace, properties: .frame)
                } else {
                    Color.clear.frame(height: 2)
                }
            }
            .animation(.spring(), value: self.currentTab)
        }
        .buttonStyle(.plain)
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

struct View1: View {
    var body: some View {
        Color.yellow.opacity(0.2).edgesIgnoringSafeArea(.all)
    }
}

struct View2: View {
    var body: some View {
        Color.blue.opacity(0.2).edgesIgnoringSafeArea(.all)
    }
}

struct View3: View {
    var body: some View {
        Color.red.opacity(0.2).edgesIgnoringSafeArea(.all)
    }
}


```
