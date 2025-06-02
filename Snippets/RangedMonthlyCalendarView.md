---
title: RangedMonthlyCalendarView
date:  2025-06-02
time:  03:52:44
tags:
  - swift
  - ios
  - swiftui
---

```swift
import SwiftUI

// ────────────────────────────────────────────────────────────────
// 1. Date 편의 확장
//    연/월/일 컴포넌트를 쉽게 가져올 수 있도록 추가합니다.
extension Date {
    var year: Int  { Calendar.current.component(.year, from: self) }
    var month: Int { Calendar.current.component(.month, from: self) }
    var day: Int   { Calendar.current.component(.day, from: self) }
}

// ────────────────────────────────────────────────────────────────
// 2. RangedMonthlyCalendarView: 재사용 가능한 “범위 내 월간 달력” 컴포넌트
//    - startDate ~ endDate 범위의 월 목록을 만들고,
//    - 각 월마다 MonthlyCalendarPageView를 렌더링.
//    - selectedDate를 @Binding으로 받아, 외부에서 상태를 제어하거나 바인딩할 수 있음.

struct RangedMonthlyCalendarView: View {
    // 외부에서 주입받을 값들
    @Binding var selectedDate: Date?     // 실제 선택된 날짜
    let startDate: Date?                 // 범위 시작일
    let endDate: Date?                   // 범위 종료일

    // 내부 상태
    @State private var selectedMonth: Date  // “현재 보고 있는 월” 단위 (매월 1일)

    // Calendar 인스턴스 (지역화, 날짜 연산에 사용)
    private let calendar = Calendar.current

    // “startDate~endDate” 사이의 매월 1일 리스트
    private var monthDates: [Date] {
        guard
            let rawStart = startDate,
            let rawEnd   = endDate,
            // 연·월 정보만 떼서 “매월 1일 자정” 형태로 정규화
            let s = calendar.date(from: calendar.dateComponents([.year, .month], from: rawStart)),
            let e = calendar.date(from: calendar.dateComponents([.year, .month], from: rawEnd))
        else {
            return []
        }

        var dates: [Date] = []
        var current = s
        while current <= e {
            dates.append(current)
            current = calendar.date(byAdding: .month, value: 1, to: current)!
        }
        return dates
    }

    // 초기화: selectedDate가 범위 내 있으면 그 날짜, 없으면 범위 첫 달 1일을 사용
    init(selectedDate: Binding<Date?>, startDate: Date?, endDate: Date?) {
        self._selectedDate = selectedDate
        self.startDate = startDate
        self.endDate   = endDate

        // rawStart 결정: startDate가 있으면 그 날짜, 아니면 첫 월 1일, 아니면 오늘
        let rawStart: Date
        if let s = startDate {
            rawStart = s
        } else if let firstMonth = {
            // 월 목록을 계산하려면 임시로 calendar를 사용해야 함
            let calendar = Calendar.current
            if
                let rs = startDate,
                let normalized = calendar.date(from: calendar.dateComponents([.year, .month], from: rs))
            {
                return normalized
            }
            return nil
        }() {
            rawStart = firstMonth
        } else {
            rawStart = Date()
        }

        // “그 달의 1일”로 정규화
        let normalized = calendar.date(
            from: calendar.dateComponents([.year, .month], from: rawStart)
        )!

        // _selectedMonth 초기값
        _selectedMonth = State(initialValue: normalized)
    }

    var body: some View {
        GeometryReader { proxy in
            VStack(spacing: 0) {
                // ── (1) 헤더: ◀︎/▶︎ 버튼 + 현재 연·월 ──
                HStack {
                    Button {
                        changeMonth(by: -1)
                    } label: {
                        Image(systemName: "chevron.left")
                            .font(.title2)
                            .padding(.vertical, 4)
                            .padding(.horizontal, 8)
                    }
                    .disabled(!hasMonth(offset: -1))

                    Spacer()

                    Text("\(selectedMonth.year)년 \(selectedMonth.month)월")
                        .font(.headline)

                    Spacer()

                    Button {
                        changeMonth(by: +1)
                    } label: {
                        Image(systemName: "chevron.right")
                            .font(.title2)
                            .padding(.vertical, 4)
                            .padding(.horizontal, 8)
                    }
                    .disabled(!hasMonth(offset: +1))

                }
                .padding(.horizontal)
                .padding(.top, 8)

                // ── (2) TabView: 스와이프 가능한 월간 달력 ──
                TabView(selection: $selectedMonth) {
                    ForEach(monthDates, id: \.self) { pageDate in
                        MonthlyCalendarPageView(
                            year: pageDate.year,
                            month: pageDate.month,
                            selectedDate: $selectedDate,
                            startDate: startDate,
                            endDate: endDate
                        )
                        .tag(pageDate)  // “매월 1일” 값과 동일해야 페이지가 선택됨
                        .frame(
                            width: proxy.size.width,
                            height: proxy.size.height * 0.85,
                            alignment: .top
                        )
                        // 배경색을 넣어 투명 공간 없애기
                        .background(Color(.systemBackground))
                    }
                }
                .tabViewStyle(PageTabViewStyle(indexDisplayMode: .never))
                .animation(.easeInOut, value: selectedMonth)
                // 월이 바뀔 때 selectedDate(일)를 조정
                .onChange(of: selectedMonth) { newMonth in
                    // (a) “그 달의 1일”을 계산
                    guard let firstOfMonth = calendar.date(
                        from: DateComponents(year: newMonth.year, month: newMonth.month, day: 1)
                    ) else { return }

                    // (b) startDate~endDate 범위 내에 있으면 1일로 선택, 아니면 nil로 해제
                    if let s = startDate, let e = endDate {
                        let cmpStart = calendar.compare(firstOfMonth, to: s, toGranularity: .day)
                        let cmpEnd   = calendar.compare(firstOfMonth, to: e, toGranularity: .day)
                        if (cmpStart == .orderedSame || cmpStart == .orderedDescending)
                            && (cmpEnd == .orderedSame || cmpEnd == .orderedAscending) {
                            // 범위 안 → 1일로 바꾸기
                            selectedDate = firstOfMonth
                        } else {
                            // 범위 밖 → 선택 해제
                            selectedDate = nil
                        }
                    } else {
                        // startDate/endDate가 nil이면, 그냥 1일로 바꾸기
                        selectedDate = firstOfMonth
                    }
                }

                Spacer(minLength: 0)
            }
            // 선택 영역 외부 배경 흰색으로 채우기
            .background(Color(.systemBackground))
        }
    }

    // “이전/다음” 버튼을 누를 수 있는지 (월 단위 비교)
    private func hasMonth(offset: Int) -> Bool {
        guard let candidate = calendar.date(
                byAdding: .month,
                value: offset,
                to: selectedMonth
        ) else { return false }
        return monthDates.contains {
            calendar.isDate($0, equalTo: candidate, toGranularity: .month)
        }
    }

    // “이전/다음” 버튼 동작: selectedMonth를 실제 monthDates 배열의 값으로 교체
    private func changeMonth(by offset: Int) {
        guard let candidate = calendar.date(
                byAdding: .month,
                value: offset,
                to: selectedMonth
        ) else { return }

        if let matched = monthDates.first(where: {
            calendar.isDate($0, equalTo: candidate, toGranularity: .month)
        }) {
            selectedMonth = matched
            // onChange(of: selectedMonth) 에서 selectedDate 처리됨
        }
    }
}

// ────────────────────────────────────────────────────────────────
// 3. MonthlyCalendarPageView: “한 달” 달력을 그리며, 날짜를 선택할 수 있음.
//    - selectedDate: 부모에서 바인딩으로 받아온 실제 날짜 (Optional).
//    - startDate/endDate 범위 밖 날짜는 회색으로 비활성화.

struct MonthlyCalendarPageView: View {
    let year: Int
    let month: Int

    @Binding var selectedDate: Date?  // 선택된 날짜를 부모에 전달
    let startDate: Date?              // 범위 시작일
    let endDate: Date?                // 범위 종료일

    @Environment(\.locale) private var locale
    private let calendar = Calendar.current
    private let weekdaySymbols = Calendar.current.shortStandaloneWeekdaySymbols

    // (a) 해당 월의 모든 날짜(1일부터 말일까지, 00:00:00으로 정규화)
    private var days: [Date] {
        guard
            let startOfMonth = calendar.date(
                from: DateComponents(year: year, month: month, day: 1, hour: 0, minute: 0, second: 0)
            ),
            let range = calendar.range(of: .day, in: .month, for: startOfMonth)
        else {
            return []
        }

        return range.map { day in
            calendar.date(
                from: DateComponents(year: year, month: month, day: day, hour: 0, minute: 0, second: 0)
            )!
        }
    }

    // (b) 로컬라이즈된 “연·월” 문자열
    private var localizedMonthYear: String {
        let components = DateComponents(year: year, month: month, day: 1)
        let date = calendar.date(from: components)!
        let format = DateFormatter.dateFormat(
            fromTemplate: "yMMMM", options: 0, locale: locale
        )!
        let df = DateFormatter()
        df.locale = locale
        df.dateFormat = format
        return df.string(from: date)
    }

    // (c) 날짜가 범위 내에 있는지 확인하는 함수
    private func isWithinRange(_ date: Date) -> Bool {
        guard let s = startDate, let e = endDate else {
            return true // startDate/endDate가 없으면 무조건 true
        }
        let cmpStart = calendar.compare(date, to: s, toGranularity: .day)
        let cmpEnd   = calendar.compare(date, to: e, toGranularity: .day)
        return (cmpStart == .orderedSame || cmpStart == .orderedDescending)
            && (cmpEnd == .orderedSame || cmpEnd == .orderedAscending)
    }

    var body: some View {
        ZStack {
            // (1) 바닥 배경
            Color(.systemBackground)
                .ignoresSafeArea()

            VStack(spacing: 12) {
                // (2) 연·월 타이틀
                Text(localizedMonthYear)
                    .font(.title2).bold()
                    .frame(maxWidth: .infinity, alignment: .leading)

                // (3) 요일 헤더 (S, M, T, W, T, F, S)
                HStack(spacing: 4) {
                    ForEach(weekdaySymbols, id: \.self) { wd in
                        Text(String(wd.prefix(1)))
                            .font(.caption2)
                            .frame(maxWidth: .infinity)
                    }
                }

                // (4) 날짜 그리드: 7열로 배열
                let columns = Array(repeating: GridItem(.flexible(), spacing: 4), count: 7)
                LazyVGrid(columns: columns, spacing: 4) {
                    // (4.1) 1일 이전 빈칸 채우기
                    if let firstDate = days.first {
                        let offset = calendar.component(.weekday, from: firstDate) - 1
                        ForEach(0..<offset, id: \.self) { _ in
                            Color.clear.frame(minHeight: 30)
                        }
                    }
                    // (4.2) 실제 날짜 셀
                    ForEach(days, id: \.self) { date in
                        let d = calendar.component(.day, from: date)
                        let allowed = isWithinRange(date)

                        Text("\(d)")
                            .font(.caption)
                            .frame(maxWidth: .infinity, minHeight: 30)
                            // 범위 밖인 경우 회색 처리, 안쪽은 기본 색
                            .foregroundColor(allowed ? .primary : .gray.opacity(0.5))
                            // 오늘/선택된 날짜 강조
                            .background(cellBackground(for: date))
                            // 탭했을 때 범위 내 날짜만 selectedDate에 할당
                            .onTapGesture {
                                if allowed {
                                    selectedDate = date
                                }
                            }
                    }
                }

                Spacer()
            }
            .padding()
            .frame(maxWidth: .infinity, maxHeight: .infinity, alignment: .top)
        }
    }

    // (d) 셀 배경: 오늘은 파란 원, 선택된 날은 파란 테두리
    @ViewBuilder
    private func cellBackground(for date: Date) -> some View {
        if calendar.isDateInToday(date) {
            Circle().fill(Color.blue.opacity(0.3))
        } else if let sel = selectedDate,
                  calendar.isDate(sel, equalTo: date, toGranularity: .day) {
            Circle().stroke(Color.accentColor, lineWidth: 2)
        } else {
            Color.clear
        }
    }
}

// ────────────────────────────────────────────────────────────────
// 4. Example Usage: ContentView에서 RangedMonthlyCalendarView를 호출

struct ContentView: View {
    @State private var pickedDate: Date? = nil

    var body: some View {
        // 예시: 2025-06-02 ~ 2025-10-15 범위
        let start: Date? = Calendar.current.date(
            from: DateComponents(year: 2025, month: 6, day: 2)
        )
        let end: Date? = Calendar.current.date(
            from: DateComponents(year: 2025, month: 10, day: 15)
        )

        VStack(spacing: 20) {
            if let d = pickedDate {
                Text("선택된 날짜: \(d.formatted(.dateTime.year().month().day()))")
                    .font(.subheadline)
            } else {
                Text("날짜를 선택해주세요")
                    .font(.subheadline)
            }

            // 재사용 가능한 달력 컴포넌트
            RangedMonthlyCalendarView(
                selectedDate: $pickedDate,
                startDate: start,
                endDate: end
            )
            .frame(height: 500)  // 원하는 높이 지정
            .cornerRadius(8)
            .shadow(radius: 2)
        }
        .padding()
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```